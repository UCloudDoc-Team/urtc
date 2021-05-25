# 云代理服务

有防火墙限制、无法直接连接互联网的环境，，如银行、学校、医院、企业内网等，使用URTC时，可以使用 **云代理服务**。    
内网客户端 通过 **云代理服务** 连接 互联网的客户端。  

## 开通云代理服务

联系 客户经理 或 技术支持 申请开通**云代理服务**，并提供以下信息：

    - AppID：    
    - 代理服务使用区域：    
    - 用户并发数：    



## 实现方法


<!-- {docsify-ignore-all} -->

<!-- tabs:start -->

## ** Web **

### 示例代码

```js
UCloudRTC.setServers({
  signal: "wss://domain:5005" // domain 为 云代理服务的域名
})
```

## ** Windows **

### 示例代码

```cpp
engine->setServerGetFrom(UCLOUD_RTC_SERVER_GET_FROM_USER_DIRECT); 
tUCloudRtcAuth  auth；
auth.mAppId = "xxx";    //your appid
auth.mRoomId = "xxx";    //your roomid
auth.mUserId = "xxx";    //your userid
auth.mUserToken = "xxx";    //就这样写
auth.mServerUrl =  "wss://domain:5005/ws";// domain 为 云代理服务的域名
engine->joinChannel(auth);
```

## ** Android **

### 示例代码

```java
 UCloudRtcSdkEnv.setPrivateDeploy(true);
 UCloudRtcSdkEnv.setPrivateDeployRoomURL("wss://domain:5005/ws"); // domain 为 云代理服务的域名
```


<!-- tabs:end -->

## 开发注意事项

在加入房间前，设置云代理服务的域名。


