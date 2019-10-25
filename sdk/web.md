

# WEB SDK指南

## 1. 下载资源

  - 可以下载Demo、SDK、API文档
  
    [现在下载](https://github.com/ucloud/urtc-js-demo.git)

## 2. 集成SDK

  - 建议使用chrome60以上版本    \\

>由于浏览器的安全策略对除 127.0.0.1 以外的 HTTP 地址作了限制，Web SDK 仅支持  HTTPS协议  或者 http://localhost（http://127.0.0.1），请勿使用  HTTP协议  部署你的项目。 


### 2.1 使用npm获取SDK

  - 可使用npm直接引入  \\

```
npm install ucloud-rtc-sdk
```

### 2.2 下载SDK包

  - [下载SDK](http://urtcsdk.cn-bj.ufileos.com/URTCSdk_web.zip)

  - 将SDK.js 文件保存到项目文件所在的目录下

  - 在项目文件中，添加如下代码：

```
<script src="urtcsdk-1.0.1.js"></script>
```

## 3. 初始化


```js
const URtcDemo = new UCloudRtcEngine();  
```

## 4. 建立通话

### 4.1.获取token  

```js
URtcDemo.getToken({
    app_id: appId,//控制台创建项目获取到的appid
    room_id: roomId,//房间号
    user_id: userId,//用户id
    appkey: appkey//控制台创建项目获取到的appkey
}).then(function(data) {
    //返回当前用户的token 
}).catch(function(err){
    //报错信息 
})
```

`appId`、`appkey`，需要填入 [快速上手](/video/urtc/quick.md) 创建的应用中的`AppID`、`AppKey`。  
`roomId`、`userId`房间号，是自定义的。


### 4.2.建立链接  

```js
URtcDemo.init({  
    app_id: appId,//控制台创建项目获取到的appid
    room_id: roomId,//房间号
    user_id: user_id,//用户id
    token: token,//getToken()获取到的token
    role_type: role_type, //用户权限0 推流 1 拉流 2 全部
    room_type: room_type //房间类型 0 rtc小班课 1 rtc 大班课 
}).then(function(data){  
//返回链接url 
}).catch(function(err){
    //报错信息 
}) 
```

`role_type`用于设置用户权限，推流、拉流，或者同时推拉流。  
`room_type`用于设置房间模式，1对1会议或者多人音频、视频通信，建议设置为rtc小班课；互动直播，建议设置rtc大班课。

### 4.3.开启本地媒体设备

```js
URtcDemo.getLocalStream({
    media_data: "videoProfile1280*720",//设置视频分辨率
    video_enable: true,//是否开始摄像头true/false
    audio_enable: true,//是否开启音频设备true/false
    media_type: 1 //采集类型 1 摄像头 2 桌面
}).then(function(data) {
    //加入媒体流
}.catch(function(err){
    //报错信息 
}) 
```

`video_enable` 和 `audio_enable` 控制是否打开音频和视频设备。


### 4.4.加入房间

```js
URtcDemo.joinRoom({
    token: token, //getToken()获取到的token
    role_type: 2, //用户权限0 推流 1 拉流 2 全部
    room_type: room_type //房间类型 0 rtc小班课 1 rtc 大班课
}).then(function(e) {
    //房间信息
}).catch(function(err){
    //报错信息 
}) 
```


### 4.5.发布本地流

```js
URtcDemo.publish({  
    user_id:user_id,//用户id  
    media_type:1,//发布的流类型 1 摄像头 2桌面  
    audio: true,//是否包含音频流 true/false
    video:true,//是否包含视频流 true/false
    data:false//是否包含数据流 true/false
}).then(function(e){  
    //发布信息 
}).catch(function(err){
    //报错信息 
});  
```

### 4.6.订阅远端流 

```js
URtcDemo.subscribe({  
 media_type: 1,//订阅的流类型 1 摄像头 2桌面  
 stream_id: “stream_id”,//订阅流id  
 user_id: “user_id",//订阅用户id  
},{  
 audio_enable: true,//是否包含音频流 true/false
 video_enable: true//是否包含数据流 true/false
}).then(function(e){  
    //订阅信息  
}).catch(function(err){
    //报错信息 
}) 
```

### 4.7.获取本地音量数据

```js
URtcDemo.getAudioVolum().then(function(e){  
     //音量数据  
 }).catch(function(err){
    //报错信息 
});
```

### 4.8.打开/关闭本地音视频

```js
URtcDemo.activeMute({  
    stream_id: stream_id,//媒流id   
    stream_type: 1,//1 发布流 2 订阅流  
    user_id: user_id,//用户id  
    track_type: 2,//1 视频 2音频  
    mute: video_enable//true 禁用 false 开启  
}).then(function(e){  
    //操作成功  
}).catch(function(err){
    //报错信息 
});

```

### 4.9.枚举本地媒体设备

```js
URtcDemo.getLocalDevices().then(function(e){  
    //成功输出本地设备数据  
    //microphones 音频输入设备列表   
    //speakers 音频输出设备列表  
    //cameras 视频输输入设备列表  
    //设备列表中的每一个元素类型相同（label：设备名称，deviceId：设备Id）   
}).catch(function(err){
    //报错信息 
});
```

### 4.10.离开房间

```js
URtcDemo.leaveRoom({  
    room_id: room_id//房间id  
}).then(function(e){  
    //退出成功  
},function(err){  
     //退出失败  
}); 
```

### 4.11.开始录制

```js
URtcDemo.startRecord({
    "mimetype": 3,//1 音频 2 视频 3 音频+视频
    "mainviewuid": appData.userId,//主窗口位置用户id
    "mainviewtype": 1,//主窗口的媒体类型 1 摄像头 2 桌面
    "width": 1280,//320~1920之间
    "height": 720,//320~1920之间
    "watermarkpos": 1, //1 左上 2 左下 3 右上 4 右下,
    "bucket": "urtc-test",
    "region": 'cn-bj' //所在区域,
}).then(function (e) {
    //返回录制文件名
    //观看录制地址规则 'http://'+ bucket + '.'+ region +'.ufileos.com/' + e.data.FileName  
}).catch(function(err){
    //错误信息
})
```

`bucket`、`region`，配置的是录制的存储位置。

>
开始录制之前，需要先在控制台上，开启录制服务，并配置可用的对象存储。


### 4.12.结束录制

```js
URtcDemo.stopRecord().then(function (e) {
    //录制成功  
}).catch(function(err){
    //错误信息
})
```
