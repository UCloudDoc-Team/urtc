# URTC 私有化环境 部署验证

## 1. URTC实时音视频的验证
#### 1.1 Web客户端
访问[Web DEMO](https://web.urtc.com.cn/)，打开【设置】，【私有化部署地址】中填入部署的音视频服务的服务器域名，格式为：wss://domain:5005。      
![](/images/priviteImage/SETweb.png)

**多个客户端输入同一个房间号码，加入会议，相互能通话，说明URTC实时音视频服务可用。**    
![](/images/priviteImage/joinroomWEB.png)

<details>
	<summary>使用UCloud的 自签证书 时 Web DEMO验证步骤</summary>

因为浏览器的安全策略，Web客户端仅支持 HTTPS 协议 或者 http://localhost ，服务对外域名如果使用UCloud的`自签证书`，则不能直接访问DEMO进行验证。    
需要按照以下步骤配置本机电脑：    
1、在本机电脑绑定`hosts`，将`URTC实时音视频服务IP`、`rtc.example.com`加入到 hosts。     
在hosts文件中，添加以下配置：   

```
# 私有化环境
URTC实时音视频服务IP  rtc.example.com
```
2、本机电脑访问：https://rtc.example.com:5005/ ，弹出安全访问警告时，选择 仍然访问这个不安全的网址。    
这时会显示包含 `Forbidden`的页面，说明本机电脑已经可以访问这个不安全的网址。      

```
{"methodtype":"","msg_id":0,"err":24130,"msg":"Forbidden"}
```
3、访问[Web DEMO](https://web.urtc.com.cn/)，打开【设置】，【私有化部署地址】中填入部署的音视频服务的服务器域名：wss://rtc.example.com:5005 。

</details>

>使用`自签证书`时，仅Web DEMO验证需要执行以上步骤，Windows、Android、iOS、macOS客户端不受此限制。        


如对接SDK，需要在SDK中设置[信令服务的访问地址](https://github.com/ucloud/urtc-sdk-web#setservers)的IP或者域名为URTC实时音视频服务的域名。    
示例如下：    
```js
UCloudRTC.setServers({
  signal: "wss://domain:5005" // domain 为 URTC 实时音视频服务的域名，domain不能用IP
})
```

#### 1.2 Windows客户端
安装[Windows DEMO](http://urtcdemo.ufile.ucloud.com.cn/umeeting_20201118_32_Install.zip)，打开【设置】，选中【私有化】，并且在输入框中，填入部署的音视频服务的服务器地址或者域名，格式为：wss://domain:5005/ws。      
![](/images//priviteImage/SETwindows.png)

**多个客户端输入同一个房间号码，加入会议，相互能通话，说明URTC实时音视频服务可用。**    
![](/images/priviteImage/joinroomWindows.png)

如对接SDK，需要设置[auth.mServerUrl](https://github.com/ucloud/urtc-win-demo/tree/private_bran/doc#class-setServerGetFrom)的domain为URTC实时音视频服务的IP或者域名。      
示例如下：    
```cpp
engine->setServerGetFrom(UCLOUD_RTC_SERVER_GET_FROM_USER_DIRECT); 
tUCloudRtcAuth  auth；
auth.mAppId = "xxx";    //your appid
auth.mRoomId = "xxx";    //your roomid
auth.mUserId = "xxx";    //your userid
auth.mUserToken = "xxx";    //就这样写
auth.mServerUrl =  "wss://domain:5005/ws";// domain 为 URTC 实时音视频服务的IP或者域名
engine->joinChannel(auth);
```
#### 1.3 Android客户端
如对接SDK，需要设置私有化环境，并设置domain为URTC实时音视频服务的IP或者域名。    
示例如下：   
```java
 UCloudRtcSdkEnv.setPrivateDeploy(true);
 UCloudRtcSdkEnv.setPrivateDeployRoomURL("wss://domain:5005/ws"); // domain 为 URTC 实时音视频服务的IP或者域名
```

## 2. URTC录制的验证
使用以上客户端，录制一段音视频内容；关闭录制后，生成录制文件。结束录制时，客户端返回的录制地址，为云存储的地址。录制服务默认将录制文件存储在本地磁盘，需要查看本地磁盘，确认录制文件是否正确的生成。        
本地文件存储路径为：http://recordServerIP:10080/record。    
**浏览器中打开以上路径，确认录制文件存在，并且录制的内容与实际相符，说明URTC录制服务可用。**
>1、录制服务的配置文件决定，录制的文件存储在公有云对象存储US3还是本地磁盘，默认是存储在本地磁盘。           
>2、如果希望录制的文件，存储在公有云对象存储US3，需要录制服务器能与公网连通且修改录制服务的配置，此时录制的文件可以通过录制结束时的地址进行回放。      

