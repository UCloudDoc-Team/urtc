# URTC私有化部署

URTC SDK除了能在公有云环境下使用，还可以在私有化部署环境中使用。    
URTC私有化服务，分为URTC实时音视频服务、URTC录制服务，2个服务需要分别部署在不同的设备上。
<!-- {docsify-ignore-all} -->
<!-- tabs:start -->

## ** 部署须知 **
### 1. 服务性能参数
URTC服务器分为：URTC实时音视频服务、URTC录制服务，均支持单机部署或者集群部署。    
单台服务器的性能如下：

|性能 |URTC实时音视频服务 |URTC录制服务 |
|:----:|:----:|:----:|
|1台2核4G服务器 | 250路 订阅流 |不合流情况下，支持50路720P单流录制<br>合流情况下，支持2路720P合流录制 |

>1、2核4G只是一个计算基准，并不是只能配置2核4G；随着机器配置增加或者集群部署机器数量增加，客户端数量倍数增加。    
>2、有录制需要时，可以存储在录制服务的本地磁盘、也可以存储在公有云对象存储US3中。    
>录制的文件大小计算方法：    
>录制 分辨率720P 码率1Mbps的视频，1个小时，录制的文件大小：1mbps x 3600s/8 = 450 MB    
>录制 分辨率360P 码率500kbps的视频，1个小时，录制的文件大小：0.5mbps x 3600s/8 = 225 MB    
>3、服务器推荐CentOS 7.2+、Ubuntu 18.04。    

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
 - Redis 缺省配置是127.0.0.1:6379，db:0，password:urtc，可根据自己网络情况调整。
 - urtc-signal、urtc-room 默认开启tls、私有化环境需自行准备域名(可绑定hosts)和自签证书。
 
### 3. 主机防火墙规则
部署私有化环境的主机，需要打开防火墙的限制，开放以下端口。    

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

>说明：    
>1、urtc-room 配置文件中缺省URTC_SN默认有3个并发的授权。若无正式授权，也可以接入3个客户端。   
>2、服务对外域名HTTPS证书和私钥，若无正常证书，可以使用自签的证书，用于测试。    
