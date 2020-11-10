# URTC私有化部署

URTC SDK除了能在公有云环境下使用，还可以在私有化部署环境中使用。URTC私有化服务，分为URTC实时音视频服务、URTC录制服务。

<!-- tabs:start -->

## ** 部署须知 **

### 1. 服务性能参数
URTC服务器分为：URTC实时音视频服务、URTC录制服务，均支持单机部署或者集群部署。    
单台服务器的性能如下：

|性能 |URTC实时音视频服务 |URTC录制服务 |
|:----:|:----:|:----:|
|1台2核4G服务器 |接入250路流 |不合流情况下，支持50路720P单流录制<br>合流情况下，支持2路720P合流录制 |

>1、2核4G只是一个计算基准，并不是只能配置2核4G；随着机器配置增加或者集群部署机器数量增加，客户端数量倍数增加。    
>2、有录制需要时，可以存储在录制服务的本地磁盘、也可以存储在公有云对象存储US3中。    
>录制的文件大小计算方法：    
>录制 分辨率720P 码率1Mbps的视频，1个小时，录制的文件大小：1mbps x 3600s/8 = 450 MB    
>录制 分辨率360P 码率500kbps的视频，1个小时，录制的文件大小：0.5mbps x 3600s/8 = 225 MB    
>3、URTC实时音视频服务推荐CentOS 7.2+、URTC录制服务推荐Ubuntu 18.04。    


### 2. 服务间调用关系

```
        - - - - - - -               - - - - - - - -
        | urtc-room |  - - - - - >  |    Redis    |
        - - - - - - -               - - - - - - - -
           ^    |                         ^
           |    |                         |               
           |    ↓                         |
       - - - - - - - -              - - - - - - - - 
       | urtc-signal | - - - - - >  | urtc-owt    |
       - - - - - - - -              - - - - - - - -
              |                           |     
              |                           |
              ↓                           ↓
        - - - - - - - -            - - - - - - - - -
        | urtc-media  |            | urtc-record |
        - - - - - - - -            - - - - - - - - -           
```

**说明：**
 - URTC实时音视频服务包含urtc-room、urtc-signal、urtc-media，URTC录制服务包含urtc-record、urtc-owt。
 - 私有化服务的注册和发现，依赖Redis。
 - urtc-room、urtc-signal 启动会向Redis 注册自身服务信息。
 - urtc-signal 服务通过urtc-room 向Redis 注册信息。
 - urtc-record、urtc-room、urtc-signal 之间的服务调用也依赖Redis服务的注册信息。
 - urtc-room、urtc-record 配置文件里定义Redis的url/port/db/password等。
 - Redis 缺省配置是127.0.0.1:6379,db:0,password:urtc,可根据自己网络情况调整。
 - urtc-signal、urtc-room 默认开启tls、私有化环境需自行准备域名(可绑定hosts)和自签证书。
 
### 3. 主机防火墙规则

|服务器|协议|端口|源地址|动作|备注|服务|
|:----:|:----:|:----:|:----:|:----:|:----:|:----:|
|URTC实时音视频服务|`TCP`|`6005`|`0.0.0.0`|`accept`|`room HTTP接口`|`RTC房间服务`|
|URTC实时音视频服务|`TCP`|`6006`|`0.0.0.0`|`accept`|`room RPC接口`|`RTC房间服务`|
|URTC实时音视频服务|`TCP`|`5005`|`0.0.0.0`|`accept`|`signal HTTP接口`|`RTC信令服务`|
|URTC实时音视频服务|`TCP`|`5006`|`0.0.0.0`|`accept`|`signal RPC接口`|`RTC信令服务`|
|URTC实时音视频服务|`TCP`|`5544`|`0.0.0.0`|`accept`|`rtsp`|`RTC媒体服务`|
|URTC实时音视频服务|`UDP`|`20000-30000`|`0.0.0.0`|`accept`|`客户端推拉流`|`RTC媒体服务`|
|URTC录制服务|`TCP`|`10008`|`0.0.0.0`|`accept`|`合流API`|`RTC合流服务`|
|URTC录制服务|`UDP`|`1-65535`|`0.0.0.0`|`accept`|`合流客户端拉流`|`RTC合流服务`|
|URTC录制服务|`UDP`|`10080`|`0.0.0.0`|`accept`|`录制视频文件回放`|`RTC录制服务`|


### 4. 服务配置所需参数汇总 

|配置项|备注|
|:----:|:----:|
|`UCloud RTC 提供的服务License`|`对应urtc-room 配置文件中缺省URTC_SN`|
|`部署主机的公网IP`|`对应urtc-room、urtc-signal、urtc-media 配置文件中缺省的URTC_PUBLICIP`|
|`部署主机的内网IP`|`对应urtc-room、urtc-signal、urtc-media 配置文件中缺省的URTC_INTRANETIP`|
|`服务对外域名`|`对应urtc-signal 配置文件中缺省的URTC_DOMAIN`|
|`服务对外域名HTTPs证书和私钥`|`对应urtc-room、urtc-signal 配置文件中缺省的cert/server.crt、cert/server.key`|

## ** 部署URTC 实时音视频服务 **

### 1. 服务安装前的准备工作
<details>
<summary>Debian/Ubuntu </summary>
        
```
    apt-get update -y
    apt-get -y install libprotobuf-dev libprotobuf-lite10 libprotobuf10 (Ubuntu18.04)          
    apt-get -y install libprotobuf-dev:amd64=2.6.1-1.3 libprotobuf-lite9v5:amd64=2.6.1-1.3 libprotobuf9v5:amd64=2.6.1-1.3 (Ubuntu16.04)`     
    apt-get -y install libssl-dev libcurl4-openssl-dev libjson-c-dev libmagic-dev libevent-dev uuid-dev autoconf automake build-essential cmake git-core libass-dev libfreetype6-dev libsdl2-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev pkg-config texinfo wget zlib1g-dev libegl1-mesa-dev libgles2-mesa-dev libmirclient-dev libmircommon-dev  nasm yasm libx264-dev libx265-dev libnuma-dev libvpx-dev libfdk-aac-dev libmp3lame-dev libopus-dev redis-server    
```
</details>  

<details>
<summary>RedHat/CentOS</summary>
        
```
   yum install -y openssl-devel.x86_64 libcurl-devel.x86_64 json-c-devel.x86_64 file-devel.x86_64 libevent-devel.x86_64 libuuid-devel.x86_64 redis.x86_64  autoconf automake bzip2 bzip2-devel cmake freetype-devel gcc gcc-c++ git libtool make mercurial pkgconfig zlib-devel libsysfs freetype-devel.x86_64 libass-devel.x86_64 libvorbis-devel.x86_64 libaom-devel.x86_64
```
</details>  


### 2. 配置并启动Redis
#### 2.1 配置Redis 
+ RedHat/CentOS 配置Redis       
1）编辑Redis的配置文件`vim /etc/redis.conf`；                        
2）找到 # requirepass foobared 去掉行首的#；        
3）foobared 替换为 urtc；    
4）保存退出。         

+ Debian/Ubuntu 配置Redis    
1）编辑Redis配置文件 vim /etc/redis/redis.conf；        
2）找到 # requirepass foobar 去掉行首的#；        
3）foobared 替换为 urtc；    
4）保存退出。    
   
#### 2.2 启动Redis
执行： `systemctl restart redis`   

#### 2.3 检查Redis状态         
执行：`systemctl status redis`
确认Redis服务启动正常之后，可以进行下一步的安装。

### 3. 安装配置并启动urtc-media
#### 3.1 安装urtc-media
+ RedHat/CentOS 安装urtc-media        
    `rpm -ivh urtc-media-$version-1.el7.x86_64.rpm`     

+ Debian/Ubuntu 安装urtc-media        
    `dpkg -i urtc-media_$version_amd64.deb`

#### 3.2 配置urtc-media  
1）编辑urtc-media配置文件 vim /home/urtc-media/conf/cfg.json；      
2）找到 URTC_INTRANETIP 替换为主机内网IP；            
3）找到 URTC_PUBLICIP 替换为主机公网IP；          
4）保存退出。          

#### 3.3 启动urtc-media      
执行 `systemctl restart urtc-media`   

#### 3.4 检查urtc-media状态    
执行  `systemctl status urtc-media`  

#### 3.5 设置urtc-media开机自启动 
执行  `systemctl enable urtc-media`


### 4. 安装配置并启动urtc-signal   
#### 4.1 安装urtc-signal 
+ RedHat/CentOS 安装urtc-signal       
执行   `rpm -ivh urtc-signal-$version-1.el7.x86_64.rpm`        

+ Debian/Ubuntu 安装urtc-signal  
执行   `dpkg -i urtc-signal_$version_amd64.deb`        

#### 4.2 配置urtc-signal     
1）编辑urtc-signal配置文件 vim /home/urtc-signal/conf/cfg.json；         
2）找到 URTC_INTRANETIP 替换为主机内网IP；              
3）找到 URTC_PUBLICIP 替换为主机公网IP；            
4）找到 URTC_DOMAIN 替换为服务对外域名；          
5）找到 cert/server.crt 替换为服务对外域名的证书；         
6）找到 cert/server.key 替换为服务对外域名的密钥；        
7）保存退出。

>需要用服务器对外域名的证书和密钥文件(自签或者证书颁发机构购买)，替换urtc-signal 缺省的/home/urtc-signal/cert的证书和密钥；否则浏览器访问时会有安全威胁的提示。    
>域名有关的内容，可以查阅[UCloud 域名服务](https://docs.ucloud.cn/udnr/README)；证书相关的内容，可以查阅[UCloud SSL证书服务](https://docs.ucloud.cn/ussl/README）。

#### 4.3 启动urtc-signal         
执行  `systemctl restart urtc-signal`

#### 4.4 检查urtc-signal状态       
执行 `systemctl status urtc-signal`

#### 4.5 设置urtc-signal开机自启动       
执行  `systemctl enable urtc-signal`


### 5. 安装配置并启动urtc-room
#### 5.1 安装urtc-room 
+ RedHat/CentOS 安装urtc-room       
    `rpm -ivh urtc-room-$version-1.el7.x86_64.rpm`        

+ Debian/Ubuntu 安装urtc-room  
    `dpkg -i urtc-room_$version_amd64.deb`        
    
#### 5.2 配置urtc-room       
1）编辑urtc-room配置文件 vim  /home/urtc-room/conf/cfg.json；     
2）找到 URTC_SN 替换为UCloud RTC 提供的License；           
3）找到 URTC_PUBLICIP 替换为主机公网IP；         
4）找到 URTC_INTRANETIP 替换为主机内网IP；        
5）找到 cert/server.crt 替换为服务对外域名的证书；       
6）找到 cert/server.key 替换为服务对外域名的密钥；    
7）保存退出。    

>需要用服务对外域名的证书和密钥文件(自签或者证书颁发机构购买)，替换urtc-room 缺省的/home/urtc-room/cert的证书和密钥；否则浏览器访问时会有安全威胁的提示。         
>域名有关的内容，可以查阅[UCloud 域名服务](https://docs.ucloud.cn/udnr/README)；证书相关的内容，可以查阅[UCloud SSL证书服务](https://docs.ucloud.cn/ussl/README）。    
 
#### 5.3 启动urtc-room       
执行  `systemctl restart urtc-room`
#### 5.4 检查urtc-room状态         
执行 `systemctl status urtc-room`
#### 5.5 设置urtc-room开机自启动      
执行 `systemctl enable urtc-room`

### 6. urtc-signal 常用测试接口

+ check 接口
执行 `curl -sk "https://127.0.0.1:5005/check" | jq`

```
{
  "server": 2,
  "ip": "本机公网IP",
  "httpPort": "5005",
  "rpcPort": "5006",
  "data": {
    "userNum": 0, 
    "inMBitsPerSec": 0.0049952,  
    "outMBitsPerSec": 0.004592,
    "qps": 0,
    "stopNewConn": false
  }
}
```

+ dump 接口
执行 `curl -sk "https://127.0.0.1:5005/dump" | jq`

```
{
  "RoomMembers": [
    "本机内网IP:6006"
  ],
  "Region": "sh",
  "Wheel": "ticker:212754 interval:2s",
  "WorkerIsCrash": false,
  "RegIsStop": 0,
  "TimeOutThreshold": 600
}
```
   
>dump 接口可以看到对应的RoomMembers，若查询不到则为Null，尝试重启服务，并检查Redis。

### 7. urtc-room 常用测试接口       
+ check 接口
执行 `curl -sk "https://127.0.0.1:6005/check" | jq`

```
{
  "ip": "本机内网IP",
  "httpPort": "6005",
  "rpcPort": "6006",
  "data": {
    "userNum": 0,
    "inMBitsPerSec": 0,
    "outMBitsPerSec": 0,
    "qps": 0,
    "stopNewConn": false
  }
}
```

+ dump 接口 
执行 `curl -sk "https://127.0.0.1:6005/dump" | jq`

```
{
  "Version": 1,
  "RootRegion": [
    "sh"
  ],
  "RegionMap": {
    "sh": []
  },
  "SignalMembers": [
    "本机公网IP:5005:URTC_DOMAIN"
  ],
  "RoomMembers": null,
  "Level": 1,
  "Region": "sh",
  "UpRegion": [
    "sh"
  ],
  "NodeMap": {}
}
```

>dump 接口可以看到SignalMembers，若查询不到则为Null，尝试重启服务，并检查Redis。

### 8. 私有化部署最终产出

上述配置完毕，产出服务端地址 https://URTC_DOMAIN:6005 供各客户端SDK调用。

### 9. 常见问题排查

+ Q:  自签证书浏览器报net::ERR_CERT_COMMON_NAME_INVALID 或者handshake异常    
  A: `若URTC_DOMAIN对应证书是自签、Web端需先浏览器访问不安全链接 https://URTC_DOMAIN:6005/uteach 添加证书信任`
 
+ Q:  Redis排查signal、room学习不到对方地址   
  A: `redis-cli -h 127.0.0.1 -p 6379 -a urtc`       
     `KEYS *`   
     
+ Q:  安装失败且dpkg -l | grep urtc 显示软件包状态未iHR      
  A: `dpkg -r 对应的软件包`        
     `编辑dpkg status文件 vim /var/lib/dpkg/status`          
     `找到并删除对应软件包的块`          
     `保存退出`      
     `apt-get update -y`     
     `重新安装该软件包即可 dpkg -i 对应的软件包`     

+ Q:  安装失败: dpkg: 处理软件包 urtc-media(这只是个例子) (--configure)时出错：该软件包正处于非常不稳定的状态；您最好在配置它之前，先重新安装它，在处理时有错误发生：urtc-media E: Sub-process /usr/bin/dpkg returned an error code (1)        
  A: `rm -rf /var/lib/dpkg/info/urtc-media*`
  

## ** 部署URTC录制服务 **

### 1. 安装配置并启动urtc-record
#### 1.1 安装urtc-record
+ RedHat/CentOS 安装urtc-record   
    `rpm -ivh urtc-record-$version-1.el7.x86_64.rpm`     

+ Debian/Ubuntu 安装urtc-record   
    `dpkg -i urtc-record_$version_amd64.deb`    
    
#### 1.2 启动urtc-record     
执行 `systemctl restart urtc-record`     
#### 1.3 检查urtc-record状态   
执行 `systemctl status urtc-record`    
#### 1.4 设置urtc-record开机自启动
执行 `systemctl status urtc-record`        

### 2. 安装配置并启动urtc-owt
#### 2.1 安装urtc-owt 
+ RedHat/CentOS 安装urtc-owt     
    `rpm -ivh urtc-owt-$version-1.el7.x86_64.rpm`     

+ Debian/Ubuntu 安装urtc-owt     
    `dpkg -i urtc-owt_$version_amd64.deb`

#### 2.2 启动urtc-owt   
执行  `systemctl restart urtc-owt`    
#### 2.3 检查urtc-owt状态     
执行  `systemctl status urtc-owt`    
#### 2.4 设置urtc-owt开机自启动
执行  `systemctl enable urtc-owt`    
      
    
## ** SDK配置私有化环境 **

### 1. URTC实时音视频的配置

- 1、Web客户端，设置[信令服务的访问地址](https://github.com/ucloud/urtc-sdk-web#setservers)的IP或者域名为URTC实时音视频服务的IP或者域名。    
```
UCloudRTC.setServers({
  signal: "wss://signalIP:5005" // signalIP 为 URTC 实时音视频服务的IP或者域名
})
```
- 2、Windows客户端，设置[auth.mServerUrl]()的IP为URTC实时音视频服务的IP。    
```
engine->setServerGetFrom(UCLOUD_RTC_SERVER_GET_FROM_USER_DIRECT); 
tUCloudRtcAuth  auth；
auth.mAppId = "xxx";
auth.mRoomId = "xxx";
auth.mUserId = "xxx";
auth.mUserToken = "xxx";
auth.mServerUrl =  "wss://ip:port/ws";
engine->joinChannel(auth);
```
- 3、Android客户端，    

### 2. URTC录制的配置
#### 2.1 SDK配置

#### 2.2 录制的回放

如果录制的文件是存储在录制服务本地磁盘，则文件存储路径为：http://recordServerIP:10080/record。
如果录制的文件，存储在公有云对象存储US3，则录制回放，可以参考[公有云录制回放](https://docs.ucloud.cn/urtc/cloudRecord/PlayRecordFile)。

<!-- tabs:end -->
