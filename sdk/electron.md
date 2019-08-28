{{indexmenu_n>18}}

# Electron SDK指南

## 1. 下载资源

  - 可以下载Demo、SDK、API Java 文档  \\
    [现在下载](https://github.com/ucloud/urtc-electron-demo.git)

## 2. 开发语言以及系统要求

  - 开发语言：C++ + javascript    \\
  - 系统要求：Windows 7 及以上版本的 Windows 系统    \\

## 3. 开发环境

### 3.1 C++ 开发：自己编译electron SDK

  - Visual Studio 2015 开发环境  \\
  - Win32 Platform  \\

### 3.2 Javascript 开发

  - 拷贝工程中UCloudRtcElectronEngine.js(java script 接口封装实现)，拷贝pulgin到自己的目录下  \\

注意：请保持路径正确，或者更改为自己的目录地址。UCloudRtcElectronEngine.js 中node文件引用路径为
./plugin/lib/release/UCloudRtcElectronEngine.node。   \\       


  - 在文件中引：import {urtcSdk} from '../ UCloudRtcElectronEngine'。   \\

## 4. 初始化

### 4.1 实现eventcallback funtion实现回调处理

```js
var eventMap={
    5000:function(){
        com.addLog('success',"ok");
    },
    5001:function () {
        com.addLog('error','服务器连接断开');
    },
    5002:function (resp,com) {
        //加入房间
    },
    5003:function (resp,com) {
        //离开房间
    },
    5004:function (resp,com) {
        //重连中
    },
    5005:function (resp,com) {
        //重连成功
    },
    5006:function (resp,com) {
        //视频发布成功
    },
    5007:function (resp,com) {
        //取消媒体 {code:0 msg:msg, data:{}}
    },
    5008:function (resp,com) {
       // 用户加入房间
    },
    5009:function (resp,com) {
       // 用户离开房间
    },
    5010:function (resp,com) {
       // 房间内新媒体发布
    },
    5011:function (resp,com) {
         //房间内有媒体流移除
    },
    5012:function (resp,com) {
        //订阅媒体流响应
   },
   5013:function (resp,com) {
        // 取消订阅媒体流响应
    },
    5014:function (resp,com) {
        // mute  本地媒体流响应
    },
    5015:function (resp,com) {
       // mute 远端媒体流响应
    },
    5016:function (resp,com) {
        // 远端媒体流变化
    },
    5019:function (resp,com) {
        //开始录制请求响应
    },
    5020:function (resp,com) {
        //停止请求录制响应
    }
}
initEngine = (eventId,objectStr)=>{
    if(!objectStr){
        objectStr = "{}";
    }
    var fun = enentMap[eventId];
    // console.log(eventId,objectStr);
    this.addLog('info',`SDK返回 ${eventId}:${objectStr}`);
    if(fun){
         fun(JSON.parse(objectStr),this);
    }
 }
```

### 4.2 调用接口初始化

```js
urtcSdk.InitRtcEngine(initEngine);
urtcSdk.SetSdkMode(1) ; 
urtcSdk.SetStreamRole(2) ;
urtcSdk.SetAudioOnlyMode(false) ;
urtcSdk.SetAutoPubSub(false, false) ;
urtcSdk.SetVideoProfile(1) ;
urtcSdk.SetScreenOutProfile(2) ;
urtcSdk.SetTokenSeckey("9129304dbf8c5c4bf68d70824462409f") ;
```

## 5. 建立通话

### 5.1 加入房间  

```js
const jsonarg = {} ;
jsonarg.uid = userid ;
jsonarg.rid = roomid ;
jsonarg.appid = "URtc-h4r1txxy" ;// test appid 
const jsonStr = JSON.stringify(jsonarg) ;
console.log("joinroom : "+ jsonStr) ;
urtcSdk.JoinRoom(jsonStr);
```

### 5.2 发布本地流  

```js
urtcSdk.PublishStream(1,this.mediaConfig.videoenable, this.mediaConfig.audioenable);
```

### 5.3 取消发布本地流  

```js
urtcSdk.UnPublishStream(1);

```

### 5.4 订阅流  

```js
const jsonarg = {} ;
jsonarg.uid = userid ;
jsonarg.audio = true ;
jsonarg.video = true; 
jsonarg.mtype = 1;
const jsonStr = JSON.stringify(jsonarg) ;
console.log("joinroom : "+ jsonStr) ;
urtcSdk. SubscribeStream (jsonStr);

```

### 5.5 取消订阅  

```js
const jsonarg = {} ;
jsonarg.uid = userid ;
jsonarg.audio = true ;
jsonarg.video = true; 
jsonarg.mtype = 1;
const jsonStr = JSON.stringify(jsonarg) ;
console.log("joinroom : "+ jsonStr) ;
urtcSdk. UnSubscribeStream (jsonStr);
```

### 5.6 离开房间  

```js
urtcSdk.LeaveRoom()
```

### 5.7 编译、运行，开始体验吧！
