# 快速集成SDK

<!-- tabs:start -->

# ** Web **

通过集成SDK，可以快速实现实时音视频通话。

## 1. Web SDK兼容性

使用一款Web SDK 兼容的浏览器，具体如下表所示：

|平台 | 浏览器 |
|-|-|
|Windows 7+  | Chrome 60+ <br> Firefox 56+ <br> Opera 50+ <br> Edge 浏览器 79+ <br> QQ 浏览器 10+ <br> 360 安全浏览器 10+ <br> 360 极速浏览器 12+  |
|macOS 10+    | Chrome 60+ <br> Firefox 56+ <br> Opera 50+  <br> Edge 浏览器 79+ <br> 360 极速浏览器 1+  <br> 苹果Safari 11+ |
|Android 5.0+  | Chrome 60+ <br> 华为手机浏览器 10+ <br> 微信浏览器 7+ |
|iOS 11+   | 苹果Safari 11+ <br> 微信浏览器 7+（仅支持接收）|

 - iOS系统的限制：iOS仅允许苹果safari 浏览器使用麦克风、摄像头设备；不允许其他浏览器使用麦克风、摄像头设备，因此iOS的微信公众号/微信浏览器中无法发布视频流，仅支持接收。

## 2. Web端Demo源码

 - 直接使用不同JS框架，接入URTC SDK的源码，具体参考 [angular、react、react Typescript、vue、纯JS demo源码](https://github.com/ucloud/urtc-sdk-web/tree/master/examples) 。
 
 - 在线教育场景[Demo源码](https://github.com/ucloud/urtc-js-demo)，Demo中集成大班课、小班课，白板，IM，连麦等功能。
 
>由于浏览器的安全策略对除 127.0.0.1 以外的 `HTTP` 地址作了限制，Web SDK 仅支持  `HTTPS` 协议  或者 `http://localhost（http://127.0.0.1）`，请勿使用  `HTTP` 协议 部署你的项目。

## 3. 引入SDK

选择如下任意一种方法引入URTC Web SDK：   

### 3.1 使用`npm`引入SDK

将 sdk 使用 ES6 语法作为模块引入。使用该方法需要先安装 `npm`，详见 [npm快速入门](https://www.npmjs.cn/getting-started/installing-node/)。

1）使用`npm`或 [`Yarn`](https://yarnpkg.com/) 集成 WEB SDK:
```
npm install --save urtc-sdk
或    
yarn add urtc-sdk
```
2）项目中引入SDK并创建 client

```
import sdk,{ Client } from 'urtc-sdk';
```
### 3.2 直接引入SDK

下载[URTC Web SDK](https://github.com/ucloud/urtc-sdk-web)，将 sdk 中 lib 目录下的 index.js 使用 script 标签引入。  

```
<script type="text/javascript" src="index.js"><script>
```
## 4. 实现音视频通话

下图展示了基础的一对一音视频通话的 API 调用：    

![](/images/sdk/VideoStartWebv2.png)


### 4.1 初始化SDK


检测当前浏览器对 WebRTC 的适配情况

```js
const result = sdk.isSupportWebRTC();
```

获取token方式。测试时使用此接口，正式使用建议调用服务端接口

```js
const token = sdk.generateToken(AppId, AppKey, RoomId, UserId);
```

加入房间之前，需要初始化，创建client。   

```js
const client = new Client(AppId, Token, {
  type?: "rtc"|"live"
  // 选填，设置房间类型，有两种 实时会议"rtc" 和互动直播"live" 类型可选 ，默认为实时会议 rtc
  role?: "pull" | "push" | "push-and-pull"
  // 选填，设置用户权限，可设 "pull" | "push" | "push-and-pull" 三种角色，分别对应拉流、推流、推+拉流，默认为 "push-and-pull"，特别地，当房间类型为实时会议（rtc）时，此参数将被忽视，会强制为 "push-and-pull"，即推+拉流
  codec?: "vp8"|"h264"
  // 选填，设置视频编码格式，可设 "vp8" 或 "h264"，默认为 "vp8"
});
```

初始化时，需注意 `type`、 `role`、`codec`参数的设置：   
 - `type`用于设置房间类型。一对一或多人通话中，建议设为 "rtc" ，使用通信场景；互动直播中，建议设为 "live"，使用直播场景。
 - `role`用于设置用户权限。在互动直播中，需要设置主播和连麦方的权限为` push-and-pull` ，不需要连麦时设置主播为 `push` ；观众设置为 `pull` 。

> 注：创建 `client` 时传的 `token` 需要使用 `AppId` 和 `AppKey` 等数据生成，测试阶段，可临时使用  [sdk](https://github.com/ucloud/urtc-sdk-web)  提供的 `generateToken` 方法生成，但为保证  `AppKey`不暴露于公网，在生产环境中强烈建议自建服务，由 [服务器按规则](https://docs.ucloud.cn/urtc/sdk/token) 生成 `token` 供 sdk 使用。

### 4.2 加入一个房间并发布本地流
```js
client.joinRoom(roomId, userId, () => {
   client.publish({
	  audio: true/false
	  // 必填，指定是否使用麦克风设备
	  video: true/false
	  // 必填，指定是否使用摄像头设备
	  screen: true/false
	  // 必填，指定是否为屏幕共享，audio, video, screen 不可同时为 true，更不可同时为 false
	  microphoneId?: ''
	  // 选填，指定使用的麦克风设备的ID，可通过 getMicrophones 方法查询获得该ID，不填时，将使用默认麦克风设备
	  cameraId?: ''
	  // 选填，指定使用的摄像头设备的ID，可以通过 getCameras 方法查询获得该ID，不填时，将使用默认的摄像头设备
	  extensionId?: ''
	  // 选填，指定使用的 Chrome 插件的 extensionId，可使 72 以下版本的 Chrome 浏览器进行屏幕共享。
	}, onFailure)
}); // 在 joinRoom 的 onSuccess 回调函数中执行 publish 发布本地流
```
### 4.3 订阅远端流
```js
client.joinRoom(roomId, userId, () => {
    client.subscribe(StreamId, onFailure)
}); // 在 joinRoom 的 onSuccess 回调函数中执行 subscribe 发布本地流
```
StreamId: string 类型，必传，为需要订阅的远端流的 流ID，类型说明见  [Stream](https://github.com/ucloud/urtc-sdk-web#stream)。

### 4.4 取消发布本地流或取消订阅远端流
```js
client.unpublish(StreamId, onSuccess, onFailure)
// 取消发布本地流
client.unsubscribe(StreamId, onSuccess, onFailure)
//取消订阅远端流
```
### 4.5 监听流事件
1、监听 "stream-published" 事件，发布成功后播放本地流。    

```js
client.on('stream-published', (stream) => {
    client.play({
        streamId: stream.sid,
        container: divElement
    });
    // 此处 divElement 代表播放发布流的外层容器元素，也可以是这个外层容器元素的 ID，而外层容器一般是一个设置了宽高的 div 元素，请根据实际情况进行传值
}); // 监听本地流发布成功事件，在当前用户执行 publish 后，与服务器经多次协商，成功后会触发此事件
```
2、 监听 "stream-added" 事件，当有远端流加入时订阅该流。    

```js
client.on('stream-added', (stream) => {
    client.subscribe(stream.sid);
}); // 监听新增远端流事件，在远端用户新发布流后，服务器会推送此事件的消息。注：当刚进入房间时，若房间已有流，也会收到此事件的通知
```

3、监听 "stream-subscribed" 事件，订阅成功后播放远端流。       

```js
client.on('stream-subscribed', (stream) => {
    client.play({
        streamId: stream.sid,
        container: divElement
    });
    // divElement 如上面所说
}); // 监听远端流订阅成功事件，在当前用户执行 subscribe 后，与服务器经多次协商，成功后会触发此事件
```
4、监听 "stream-removed" 事件，当远端流被移除时（停止发布、关闭网页等）， 停止播放该流并移除它的画面。    

```js
client.on('stream-removed', (stream) => {
    // 在页面中删除播放该流的外层容器元素
}); // 监听移除的远端流事件，在远端用户取消推流或流已关闭时，服务器会推送此事件的消息。

```

5、监听 "stream-reconnected" 事件，当网络切换、网络故障等导致远端流重新连接时，直接删除原来的流，并播放新的流。    

```js
client.on('stream-reconnected', (streams) => {
    const { previous, current } = streams;
    // 从本地缓存的流的数据里找到对应的流并用重连后该流的数据更新原流的数据，或直接删除原来的流，并使用新的流
    let idx = allStreams.findIndex(item => item.sid === previous.sid);
    if (idx >= 0) {
        allStreams.splice(idx, 1, current);
    }
    client.play({
        streamId: current.sid,
        container: divElement
    });
}); // 当网络断开又恢复时，发布/订阅流可能会被重连，重连成功后，会通过此事件通知业务侧
```

### 4.6 退出房间
```js
client.leaveRoom();
```
### 4.7 开始体验吧！


# ** Windows **

通过集成SDK，可以快速实现实时音视频通话。

## 1. 下载资源

  - 可以下载Demo、SDK、API文档  
  
    [现在下载](https://github.com/ucloud/urtc-win-demo.git)  

## 2. 开发语言以及系统要求

  - 开发语言：C++  
  - 系统要求：Windows 7 及以上版本的 Windows 系统  

## 3. 开发环境

  - Visual Studio 2015 及其它c++ 开发环境  
  - Win32 Platform  

## 4. 搭建开发环境

  - 导入 SDK    
  
1） 将 sdk/include 目录添加到项目的 INCLUDE 目录下。    
2） 将 sdk/lib 目录放入项目的 LIB 目录下。  
3） 将 sdk/dll 下的 dll 文件复制到你的可执行文件所在的目录下。  


## 5. 实现音视频通话

### 5.1 初始化

```cpp
Class UcloudRtcEventListenerImpl ： public UcloudRtcEventListener {
……
};
UcloudRtcEventListener* eventhandler = new UcloudRtcEventListenerImpl

m_rtcengine = UCloudRtcEngine::sharedInstance(eventhandler);
m_rtcengine->setChannelTye(UCLOUD_RTC_CHANNEL_TYPE_COMMUNICATION);
//设置房间类型:实时通话、互动直播
m_rtcengine->setSdkMode(UCLOUD_RTC_SDK_MODE_TRIVAL);
//设置测试模式、正式模式
m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
//互动直播模式下，设置用户权限
m_rtcengine->setTokenSecKey(TEST_SECKEY);
//测试模式下设置自己的秘钥
m_rtcengine->setAudioOnlyMode(false);
//设置仅音频模式
m_rtcengine->setAutoPublishSubscribe(false, true);
//设置是否自动订阅
m_rtcengine->configLocalAudioPublish(false)；
//设置是否自动发布
m_rtcengine->configLocalCameraPublish(true);
//设置摄像头是否可以发布
m_rtcengine->configLocalScreenPublish(false);
//设置屏幕是否可以发布
tUCloudVideoConfig& videoconfig
m_rtcengine->setVideoProfile(UCLOUD_RTC_VIDEO_PROFILE_640_360，videoconfig); 
// 设置视频编码参数，UCLOUD_RTC_VIDEO_PROFILE_NONE 时 后面填入自定义编码参数  最大1080p(1920*1080)
```
初始化时，需注意 `setChannelTye`、 `setStreamRole`、参数的设置：   
 - `setChannelTye`用于设置房间类型。一对一或多人通话中，建议设为 `UCLOUD_RTC_CHANNEL_TYPE_COMMUNICATION` ，使用通信场景；互动直播中，建议设为 `UCLOUD_RTC_CHANNEL_TYPE_BROADCAST`，使用直播场景。
 - `setStreamRole`用于设置用户权限。在互动直播中，需要设置主播和连麦方的权限为`UCLOUD_RTC_USER_STREAM_ROLE_BOTH` ，不需要连麦时设置主播为 `UCLOUD_RTC_USER_STREAM_ROLE_PUB` ；观众设置为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB` 。


### 5.2 加入房间

```cpp
tUCloudRtcAuth auth;
auth.mAppId = appid;
auth.mRoomId = roomid;
auth.mUserId = userid;
auth.mUserToken = your generate token;
m_rtcengine->joinChannel(auth);
```

### 5.3 发布流

```cpp
tUCloudRtcMediaConfig config;
config.mAudioEnable = true;
config.mVideoEnable = true;
m_rtcengine->publish(UCLOUD_RTC_MEDIATYPE_VIDEO, config.mVideoEnable,config.mAudioEnable);
```

### 5.4 取消发布

```cpp
tUCloudRtcVideoCanvas view;
view.mVideoView = (int)m_localWnd->GetVideoHwnd();
view.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;		
m_rtcengine->stopPreview(view);
m_rtcengine->unPublish(UCLOUD_RTC_MEDIATYPE_VIDEO);
``` 

### 5.5 订阅流
```cpp
m_rtcengine->subscribe(tUCloudRtcStreamInfo & info)
```

### 5.6 取消订阅

```cpp
m_rtcengine->unSubscribe(tUCloudRtcStreamInfo& info)
```


### 5.7 离开房间

```cpp
m_rtcengine->leaveChannel()
```

### 5.8 编译、运行，开始体验吧！


# ** Android **

通过集成SDK，可以快速实现实时音视频通话。

## 1. 下载资源

  - 可以下载Demo、SDK、API 接口文档  
    [现在下载](https://github.com/ucloud/urtc-android-demo)

## 2. 开发语言以及系统要求

  - 开发语言：Java
  - 系统要求：Android 4.1及以上版本的移动设备
  - Android SDK API：等级 16 或以上
  - ABI支持：armeabi-v7a、arm64-v8a

## 3. 开发环境

  - android studio 需要android SDK

## 4. 搭建开发环境

  - 下载urtc android SDK包，SDK包为aar格式，名称为`ucloudrtclib`开头加版本号加一串8位识别码，可以参考github上的接入demo。   
  - 将aar 文件拷贝到自己的`lib` 目录下，然后添加到`lib` 中，修改要使用sdk模块目录下`build.gradle`，确保已经添加了如下依赖，如下所示：

```java
    dependencies {
    implementation (name: 'ucloudrtclib_1.0.1_b52bc04c', ext: 'aar')
```
  - 如果项目混淆，请在混淆中添加一下urtc 混淆规则。

```java
-keep class com.ucloudrtclib.sdkengine.**{*;}
-keep class com.ucloudrtclib.sdkengine.define.*{*;}
-keep enum com.ucloudrtclib.sdkengine.define.*{*;}

-keepclassmembers class com.ucloudrtclib.sdkengine.UCloudRtcSdkEnv {
    public static <methods>;
}

-keepclassmembers interface com.ucloudrtclib.sdkengine.UCloudRtcSdkEngine {
    public <methods>;
    public static <methods>;
}
-keep class org.webrtc.** {
    *;
}


```

  - 目录结构

![](/images/sdk/aar.png)

  - 添加权限

在 Android 6.0 (API 23)开始，用户需要在应用运行时授予权限，而不是在应用安装时授予，并分为正常权限和危险权限两种类型。    
在实时音视频SDK 中，用户需要在进入音视频通话房间前动态申请 `CAMERA`、`RECORD\_AUDIO`、`WRITE\_EXTERNAL\_STORAGE`权限，具体可以参考[Android官方文档](https://developer.android.com/training/permissions/requesting?hl=zh-cn)。

```java
<uses-feature android:name="android.hardware.camera" />
<uses-feature android:name="android.hardware.camera.autofocus" />
<uses-feature android:glEsVersion="0x00020000" android:required="true" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.BLUETOOTH" />

```

## 5. 初始化

### 5.1 引擎环境初始化

主要配置`android context sdkmode`以及`AppID` ，测试用的`SEC\_KEY`，日志等级。

```java
public class UCloudRtcApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate: " + this);
        if (TextUtils.equals(getCurrentProcessName(this), getPackageName())) {
            init();//判断成功后才执行初始化代码
        }
    }

    private void init(){
	sContext = this;
	//初始化sdk环境
	UCloudRtcSdkEnv.initEnv(getApplicationContext());
	//打印日志到logcat
	UCloudRtcSdkEnv.setWriteToLogCat(true);
	//开启log上报
	UCloudRtcSdkEnv.setLogReport(true);
	//设置log级别
	UCloudRtcSdkEnv.setLogLevel(UCloudRtcSdkLogLevel.UCLOUD_RTC_SDK_LogLevelInfo);
	//设置sdk模式（测试模式）
	UCloudRtcSdkEnv.setSdkMode(UCloudRtcSdkMode.UCLOUD_RTC_SDK_MODE_TRIVAL);
	//重连次数
	UCloudRtcSdkEnv.setReConnectTimes(60);
	//设置测试模式的用户私有秘钥
	UCloudRtcSdkEnv.setTokenSeckey(CommonUtils.SEC_KEY);

	WindowManager windowManager = (WindowManager) getSystemService(Context.WINDOW_SERVICE);
	DisplayMetrics outMetrics = new DisplayMetrics();
	windowManager.getDefaultDisplay().getMetrics(outMetrics);
	CommonUtils.mItemWidth = (outMetrics.widthPixels - UiHelper.dipToPx(this, 15)) / 3;
	CommonUtils.mItemHeight = CommonUtils.mItemWidth;
	//初始化bugly日志
	CrashReport.initCrashReport(getApplicationContext(), "9a51ae062a", true);
    }
}
```

### 5.2 继承实现`UCloudRtcSdkEventListener` 实现事件处理


```java
UCloudRtcSdkEventListener eventListener = new UCloudRtcSdkEventListener() {
        @Override
        public void onServerDisconnect() {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    ToastUtils.shortShow(RoomActivity.this, " 服务器已断开");
                    stopTimeShow();
                    onMediaServerDisconnect() ;
                }
            });
        }
    
        @Override
        public void onJoinRoomResult(int code, String msg, String roomid) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    if (code == 0) {
                        ToastUtils.shortShow(RoomActivity.this, " 加入房间成功");
                        startTimeShow();
                    }else {
                        ToastUtils.shortShow(RoomActivity.this, " 加入房间失败 "+
                                code +" errmsg "+ msg);
                        Intent intent = new Intent(RoomActivity.this, ConnectActivity.class);
                        onMediaServerDisconnect() ;
                        startActivity(intent) ;
                        finish();
                    }
    
                }
            });
        }
```

### 5.3 获取SDK 引擎 并进行基础配置

```java
sdkEngine.setAudioOnlyMode(true) ; 
// 设置纯音频模式
sdkEngine.configLocalCameraPublish(false) ; 
// 设置摄像头是否发布
sdkEngine.configLocalAudioPublish(true) ; 
// 设置音频是否发布，用于让sdk判断自动发布的媒体类型
sdkEngine.configLocalScreenPublish(false) ; 
// 设置桌面是否发布，作用同上
sdkEngine.setClassType(UCloudRtcSdkRoomType.UCLOUD_RTC_SDK_ROOM_SMALL) ; 
// 设置房间类型，有两种 实时会议（小班课） 和互动直播（大班课）类型可选 ，默认为实时会议（小班课）
sdkEngine.setStreamRole(URTCSdkStreamRole.URTC_SDK_STREAM_ROLE_BOTH);
// 如果是互动直播（大班课）模式，需要设置用户权限：仅上行发布、仅下行订阅、双向发布订阅权限；实时会议（小班课）会忽略这个配置
sdkEngine.setAutoPublish(true) ; 
// 是否自动发布
sdkEngine.setAutoSubscribe(true) ;
// 是否自动订阅
sdkEngine.setVideoProfile(UCloudRtcSdkVideoProfile.matchValue(mVideoProfile)) ;
// 摄像头输出等级
```

## 6. 实现音视频通话

### 6.1 加入房间

```java
UCloudRtcSdkAuthInfo info = new UCloudRtcSdkAuthInfo();
        info.setAppId(mAppid);
        info.setToken(mRoomToken);
        info.setRoomId(mRoomid);
        info.setUId(mUserid);
        Log.d(TAG, " roomtoken = " + mRoomToken);
        sdkEngine.joinChannel(info); 
```

### 6.2 发布媒体流

  - 如果配置了自动发布无需调用发布视频接口，SDK会在用户成功加入房间后自动发布，只需要监听事件调用渲染接口即可。

```java
sdkEngine.setAutoPublish(mPublishMode == CommonUtils.AUTO_MODE ? true : false);
```

  - 如果配置了手动发布需要调用`sdkEngine`引擎的`publish`接口 配置手动/自动发布。


```java
sdkEngine.publish(UCloudRtcSdkMediaType mtype, boolean hasvideo, boolean hasaudio)
//回调事件
public void onLocalPublish(int code, String msg, UCloudRtcSdkStreamInfo info
```


  - 媒体发布类型

现在的类型包括两大类，需要传入`publish`接口的`mtype`,`hasvideo`,`hasaudio`参数各不相同，混合类型是单一类型的组合，具体代码可参阅urtcdemo的`RoomActvity`中的处理。 

 - 混合类型：音频+屏幕捕捉、视频+屏幕捕捉   
 - 单一类型        
   1） 音频（mtype:urtc_sdk_media_type_video,hasvideo:false,hasaudio:true）    
   2） 视频（mtype:urtc_sdk_media_type_video,hasvideo:true,hasaudio:true）    
   3） 屏幕捕捉（mtype:urtc_sdk_media_type_screen,hasvideo:true,hasaudio:false）    



  - 渲染本地媒体流

在`onLocalPublish` 回调成功后，在函数中可以调用视频渲染。

```java
localrenderview.setBackgroundColor(Color.TRANSPARENT);
sdkEngine.startPreview(info.getmMediatype(), localrenderview);
//不想渲染时可以调用停止渲染接口
sdkEngine.stopPreview(UCloudRtcSdkMediaType mediatype
```
  - 取消发布媒体流

```js
sdkEngine.unPublish(UCloudRtcSdkMediaType mtype)
//回调事件
public void onLocalUnPublish(int code, String msg, UCloudRtcSdkStreamInfo info
```



### 6.3 订阅媒体流

如果配置了自动订阅无需调用订阅视频接口，SDK会在用户成功加入房间后查看房间已有的可以订阅的流并进行逐一订阅，当有新用户加入房间时也会自动订阅他推的流。   
如果配置了手动订阅需要调用sdkEngine引擎的subscribe接口。 

```java
sdkEngine.setAutoSubscribe(mScribeMode == CommonUtils.AUTO_MODE ? true : false);
```

  - 订阅媒体流

```java
sdkEngine.subscribe(UCloudRtcSdkStreamInfo info)
//回调事件
public void onSubscribeResult(int code, String msg, UCloudRtcSdkStreamInfo info
```

  - 渲染订阅的媒体流

在onSubscribeResult回调成功后，再函数中可以调用视频渲染。

```java
sdkEngine. startRemoteView(UCloudRtcSdkStreamInfo info, UCloudRtcSdkSurfaceVideoView renderview)
//不想渲染时可以调用定制渲染接口
sdkEngine.stopPreview(UCloudRtcSdkMediaType mediatype
```

  - 取消订阅媒体流

```java
sdkEngine. subscribe(UCloudRtcSdkStreamInfo info) 
//回调事件
public void onUnSubscribeResult(int code, String msg, UCloudRtcSdkStreamInfo info)
```

### 6.4 用户发布和订阅的权限控制

权限分为发布，订阅，全部权限，全部权限包括了发布和订阅。

```java
//接口
public int setStreamRole(UCloudRtcSdkStreamRole role)
//调用
sdkEngine.setStreamRole(mRole);
```
   
  
### 6.5 离开房间


```java
sdkEngine.leaveChannel() ;
```

### 6.6 编译、运行，开始体验吧！

# ** iOS **

通过集成SDK，可以快速实现实时音视频通话。

## 1. 下载资源

* 可以下载 Demo、SDK、API文档  
  [现在下载](https://github.com/ucloud/urtc-ios-demo.git)


## 2. 开发语言以及系统要求

  - 支持语言：objective-c、swift;  
  - Apple设备：iPhone最低支持iPhone5；  
  - 系统版本：最低支持iOS 9.0；  
  - CPU架构：支持arm64、armv7、x86架构，支持真机和模拟器运行；   
  - 其他：v1.5.8及以后版本支持bitcode。
  

## 3. 开发环境  

  - Xcode 9.0及以上版本；  
  - Apple开发证书或个人账号；  


## 4. 搭建开发环境  


### 4.1 得到动态库

下载SDK,得到的UCloudRtcSdk\_ios.framework为动态库；  

### 4.2 创建新的工程

使用XCode创建一个新的工程UCloudRtcSdk-ios-demo；  

![创建新的工程.png](/images/sdk/%E5%88%9B%E5%BB%BA%E6%96%B0%E7%9A%84%E5%B7%A5%E7%A8%8B.png)

### 4.3 加入动态库带工程中

将已下载的动态库**UCloudRtcSdk\_ios.framework**加入到**UCloudRtcSdk-ios-demo**工程中**Embedded Binaries**；  

![加入动态库到工程中](/images/sdk/%E5%8A%A0%E5%85%A5%E5%8A%A8%E6%80%81%E5%BA%93%E5%88%B0%E5%B7%A5%E7%A8%8B%E4%B8%AD.png)

### 4.4 打开Xcode

打开Xcode，选择：项目TARGET -\>General-\>Deployment Target，设置8.0或以上版本；  

![设置版本号.png](/images/sdk/%E8%AE%BE%E7%BD%AE%E7%89%88%E6%9C%AC%E5%8F%B7.png) 

### 4.5 使用动态库不需要添加其他库依赖

### 4.6 关闭Bitcode（目前SDK版本不支持Bitcode） 

![关闭bitcode.png](/images/sdk/%E5%85%B3%E9%97%ADbitcode.png) 

### 4.7 编辑info.plist，申请摄像头、麦克风权限

Privacy - Camera Usage Description  
Privacy - Microphone Usage Description  

![编辑info.plist.png](/images/sdk/%E7%BC%96%E8%BE%91info.plist.png) 

### 4.8 打开后台音频权限

为保障APP退入手机后台之后，通话可以保持不中断，建议开启后台音频权限，SDK默认进入后台之后继续推送音频流。

![打开后台音频权限.png](/images/sdk/%E6%89%93%E5%BC%80%E5%90%8E%E5%8F%B0%E9%9F%B3%E9%A2%91%E6%9D%83%E9%99%90.png) 

### 4.9 集成成功

按照上述步骤完成UCloudRtcSdk-ios-demo的前期SDK集成准备之后，请使用Xcode连接iPhone真机，在真机调试环境下，执行编译Commond + B，提示Build Success，表示SDK集成成功。  

## 5. 初始化

建议在初始化 App 的同时，初始化 SDK。  

### 5.1 导入 SDK 头文件  

```objectivec
<UCloudRtcSdk_ios/UCloudRtcSdk_ios.h> 
```   

```swift
import UCloudRtcSdk_ios
```

### 5.2 设置 userId 和 roomId，获取AppID;  

```objectivec
UCloudRtcEngine *engine = [[UCloudRtcEngine alloc] initWithUserId:userId  appId:appId roomId:roomId appKey:appKey token:token]];
```

```swift
UCloudRtcEngine *engine = UCloudRtcEngine.init(userId:userId, appId: appId, roomId:roomId , appKey: appKey, token:token)
```

务必要设置代理对象，并实现代理回调方法，设置代理对象失败，会导致 App 收不到相关回调。

```objectivec
engine.delegate = self;
```

```swift
self.engine?.delegate = self
```

### 5.3 配置参数 

使用之前需要对SDK进行相关设置，如果不设置，系统将会采用默认值。      
初始化完成后，即可调用 SDK 相关接口，实现对应功能。     

```objectivec
    self.engine.isAutoPublish = YES;//加入房间后将自动发布本地音视频 默认为YES
    self.engine.isAutoSubscribe = YES;//加入房间后将自动订阅远端音视频 默认为YES
    self.engine.isOnlyAudio = NO;//将启用纯音频模式 默认为NO
    self.engine.isDebug = NO;//是否开启日志
    self.engine.videoProfile = UCloudRtcEngine_VideoProfile_360P_1;//设置视频分辨率
    self.engine.streamProfile = UCloudRtcEngine_StreamProfileAll;//设置流权限
```    
```swift
    self.engine?.isAutoPublish = ture;//加入房间后将自动发布本地音视频 默认为ture
    self.engine?.isAutoSubscribe = ture;//加入房间后将自动订阅远端音视频 默认为ture
    self.engine?.isOnlyAudio = false;//将启用纯音频模式 默认为false
    self.engine?.isDebug = false;//是否开启日志
    self.engine?.videoProfile = ._VideoProfile_360P_1;//设置视频分辨率
    self.engine?.streamProfile = .streamProfileAll;//设置流权限
```

## 6. 实现音视频通话

### 6.1 加入房间

```objectivec
[self.engine joinRoomWithcompletionHandler:^(NSData *data, NSUrlResponse *response, NSError error) {
    }];
```  

```swift 
self.engine?.joinRoomWithcompletionHandler({(data, response, error) -> Void in})
```

### 6.2 发布本地流  

1）自动发布模式下，joinRoom成功后，即可发布本地流，无需再次调用publish接口；    
2）手动发布模式下，joinRoom成功后，可通过下述接口发布本地流；

```objectivec
[self.engine publish];
``` 

```swift
self.engine?.publish()
```

3）发布过程中可以监听以下事件获取发布状态，根据状态调用渲染或其他接口即可。    

```objectivec
        - (void)uCloudRtcEngine:(UCloudRtcEngine *)manager didChangePublishState:(UCloudRtcEnginePublishState)publishState {
            switch (publishState) {
                        case UCloudRtcEnginePublishStateUnPublish:
                            self.isConnected = NO;
                        break;
                        case UCloudRtcEnginePublishStatePublishing: {
                            [self.bottomButton setTitle:@"正在发布..." forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishSucceed:{
                            self.isConnected = YES;
                            [self.view makeToast:@"发布成功" duration:1.5 position:CSToastPositionCenter];
                            [self.bottomButton setTitle:@"发布成功" forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStateRepublishing: {
                            [self.bottomButton setTitle:@"正在重新发布..." forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishFailed: {
                        self.isConnected = NO;
                            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
                        }
                        break;
                        case UCloudRtcEnginePublishStatePublishStoped: {
                        self.isConnected = NO;
                            [self.view makeToast:@"发布已停止" duration:1.5 position:CSToastPositionCenter];
                            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
                        }
                        break;
                        default:
                        break;
                    }                               
                }
```

```swift
        func uCloudRtcEngine(_ manager: UCloudRtcEngine, didChange publishState: UCloudRtcEnginePublishState) {
            switch publishState {
                case .unPublish:
                    self.isConnected = false
                case .publishing:
                    CBToast.showToastAction(message: "正在发布...")
                case .publishSucceed:
                    CBToast.showToastAction(message: "发布成功")
                    self.isConnected = true;
                    self.bottomButton?.setTitle("发布成功", for: .normal)
                case .republishing:
                    self.bottomButton?.setTitle("正在重新发布...", for: .normal)
                case .publishFailed:
                    self.isConnected = false;
                    CBToast.showToastAction(message: "开始发布")
                case .publishStoped:
                    self.isConnected = false;
                    CBToast.showToastAction(message: "发布已停止")
                    self.bottomButton?.setTitle("开始发布", for: .normal)
                default:
                break
            }
        }
```


### 6.3 取消发布本地流  

```objectivec
[self.engine unPublish];
```  

```swift
self.engine?.unPublish()
``` 

### 6.4 订阅远端流  

1）自动订阅模式下，joinRoom成功后，即可订阅远程流，无需再次调用subscribeMethod接口；    
2）手动订阅模式下，joinRoom成功后，可通过下述接口订阅远程流；   

```objectivec
[self.engine subscribeMethod:remoteStream];
```

```swift
self.engine?.subscribeMethod(remoteStream)
```

3）订阅成功，在回调事件中调用渲染接口即可。  

```objectivec
        -(void)uCloudRtcEngine:(UCloudRtcEngine *)channel didSubscribe:(UCloudRtcStream *)stream{
            [self reloadVideos];
        }
```

```swift
        func uCloudRtcEngine(_ channel: UCloudRtcEngine, didSubscribe stream: UCloudRtcStream) {
            self.reloadVideos()
        }
}
```

### 6.5 取消订阅远端流

```objectivec
[self.engine unSubscribeMethod:remoteStream];
```

```swift
self.engine?.unSubscribeMethod(remoteStream)
```


### 6.6 离开房间

```objectivec
[self.engine leaveRoom];   
    
```

```swift
self.engine?.leaveRoom()
```

### 6.7 编译、运行，开始体验吧！


# ** macOS **

通过集成SDK，可以快速实现实时音视频通话。


## 1. 下载资源

  - 可以下载 Demo、SDK、API文档    
  
    [现在下载](https://github.com/ucloud/urtc-mac-demo.git)

## 2. 开发语言以及系统要求

  - 系统版本：macOS 10.0；           


## 3. 开发环境  

  - Xcode 9.0及以上版本； 

  - Apple开发证书或个人账号；    


## 4. 搭建开发环境  

### 4.1 得到动态库

下载SDK,得到的UCloudRtcSdk\_mac.framework为动态库；    

### 4.2 创建新的工程

使用XCode创建一个新的工程UCloudRtcSdk-mac-demo；   

![](/images/sdk/MACOS/4.2.png)

### 4.3 加入动态库到工程中

将已下载的动态库UCloudRtcSdk\_mac.framework加入到UCloudRtcSdk-mac-demo工程中Embedded Binaries；    

![](/images/sdk/MACOS/4.3.png)

### 4.4 打开Xcode

将TARGETS>GENERAL>Deployment Target 设置为10.10及以上；

![](/images/sdk/MACOS/4.4.png)

### 4.5 编辑info.plist，申请摄像头、麦克风权限

Privacy - Camera Usage Description    
Privacy - Microphone Usage Description    

![](/images/sdk/MACOS/4.5.png)

### 4.6 打开网络请求相关权限

![](/images/sdk/MACOS/4.6.png)

### 4.7 集成成功

按照上述步骤完成UCloudRtcSdk-mac-demo的前期SDK集成准备之后，执行编译
Commond + B，提示Build Success，表示SDK集成成功。  

## 5. 初始化

建议在初始化 App 的同时，初始化 SDK。  

### 5.1 导入 SDK 头文件  

```objectivec
<UCloudRtcSdk_mac/UCloudRtcSdk_mac.h>
```

### 5.2 设置 userId 和 roomId，获取AppID 

```objectivec
UCloudRtcEngine *engine = [[UCloudRtcEngine alloc]
initWithUserId:userId appId:appId roomId:roomId token:@""]];
```

务必要设置代理对象，并实现代理回调方法，设置代理对象失败，会导致 App 收不到相关回调。

```objectivec
engine.delegate = self;
```

### 5.3 调用接口初始化

使用之前需要对SDK进行相关设置，如果不设置，系统将会采用默认值。  

```objectivec
self.engineMode = UCloudRtcEngineModeTrival; 默认为测试模式
self.engine.isAutoPublish = YES;//加入房间后将自动发布本地音视频 默认为YES
self.engine.isAutoSubscribe = YES;//加入房间后将自动订阅远端音视频 默认为YES
self.engine.isDesktop = NO;//发布桌面或者摄像头 默认为NO:摄像头 YES:桌面
```

## 6. 实现音视频通话

### 6.1 加入房间

```objectivec
[self.engine joinRoomWithcompletionHandler:^(NSData *data, NSUrlResponse *response, NSError error) {
}];

```

### 6.2 发布本地流  

1）自动发布模式下，joinRoom成功后，随即发布本地流；      

2）发布过程中可以监听以下事件获取发布状态，根据状态调用渲染或其他接口即可。      

```objectivec
- (void)uCloudRtcEngine:(UCloudRtcEngine *)manager didChangePublishState:(UCloudRtcEnginePublishState)publishState {
    switch (publishState) {
        case UCloudRtcEnginePublishStateUnPublish:
            self.isConnected = NO;
            break;
        case UCloudRtcEnginePublishStatePublishing: {
            [self.bottomButton setTitle:@"正在发布..." forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishSucceed:{
            self.isConnected = YES;
            [self.view makeToast:@"发布成功" duration:1.5 position:CSToastPositionCenter];
            [self.bottomButton setTitle:@"发布成功" forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStateRepublishing: {
            [self.bottomButton setTitle:@"正在重新发布..." forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishFailed: {
            self.isConnected = NO;
            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishStoped: {
            self.isConnected = NO;
            [self.view makeToast:@"发布已停止" duration:1.5 position:CSToastPositionCenter];
            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
        }
            break;
        default:
            break;
    }
}
```

### 6.3 订阅远程流  

1）自动订阅模式下，joinRoom成功后，即可订阅远程流；    

2）订阅成功，在回调事件中调用渲染接口即可。  

```objectivec
-(void)uCloudRtcEngine:(UCloudRtcEngine *)channel didSubscribe:(UCloudRtcStream *)stream{
     [self reloadVideos];
}
```

### 6.4 离开房间

```objectivec
[self.engine leaveRoom];
```

### 6.5 编译、运行，开始体验吧！

# ** Electron **

通过集成SDK，可以快速实现实时音视频通话。

## 1. 下载资源

  - 可以下载Demo、SDK、API 接口文档     
    [现在下载](https://github.com/ucloud/urtc-electron-demo.git)

## 2. 开发语言以及系统要求

  - 开发语言：C++ + javascript       
  - 系统要求：Windows 7 及以上版本的 Windows 系统    

## 3. 开发环境

### 3.1 C++ 开发：自己编译electron SDK

  - Visual Studio 2015 开发环境  
  - Win32 Platform  

### 3.2 Javascript 开发

  - 拷贝工程中UCloudRtcElectronEngine.js(java script 接口封装实现)，拷贝pulgin到自己的目录下。  

>注意：请保持路径正确，或者更改为自己的目录地址。UCloudRtcElectronEngine.js 中node文件引用路径为
./plugin/lib/release/UCloudRtcElectronEngine.node。   


  - 在文件中引：import {urtcSdk} from '../ UCloudRtcElectronEngine'。   

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

## 5. 实现音视频通话

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

# ** Linux Ubuntu **

通过集成SDK，可以快速实现实时音视频通话。


## 1. 下载资源

  - 可以下载Demo、SDK、API文档  
  
    [现在下载](https://github.com/ucloud/urtc-linux-demo/tree/rtsp)  

## 2. 开发语言以及系统要求

  - 开发语言：C++  
  - 系统要求：Linux Ubuntu 16.04、18.04  

## 3. 编译工具

  - cmake make gcc g++

URTC 以动态链接库的方式提供SDK，包括头文件和动态链接库文件：    
urtclib/interface/UCloudRtcComDefine.h urtclib/interface/UCloudRtcEngine.h urtclib/interface/UCloudRtcErrorCode.h     urtclib/interface/UCloudRtcMediaDevice.h urtclib/lib/libliburtcmediaengine.so urtclib/lib/liburtcnetengine.so    

## 4. RTSP源的约束

视频源为RTSP格式时，只支持H.264 baseline，RTSP 关键帧(GOP)设置推荐在3秒以内，码率设置需要小于3000kbps。

## 5. 编译demo

### 5.1 在目标机器上编译

如果在目标机器上编译使用下面的命令：
```cpp
cd build
//生成Makefile等
cmake ../.
//编译成功后，在../bin中生成可执行文件
make
//执行
cd ..
bin/enginedemo rtsp://path/to/rtspstream
```

### 5.2 交叉编译
如果使用交叉编译，需要修改CMakeList.txt，这样交叉编译速度会有所提高。
```cpp
//打开文件CMakeList.txt中下面的注释
SET(CROSS_COMPILE 1)

IF(CROSS_COMPILE)
SET(TOOLCHAIN_DIR "/path/to/compile-toolchain/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu")
set(CMAKE_CXX_COMPILER ${TOOLCHAIN_DIR}/bin/aarch64-linux-gnu-g++)
set(CMAKE_C_COMPILER   ${TOOLCHAIN_DIR}/bin/aarch64-linux-gnu-gcc)

SET(CMAKE_FIND_ROOT_PATH  ${TOOLCHAIN_DIR}
 ${TOOLCHAIN_DIR}/include
 ${TOOLCHAIN_DIR}/lib )
ENDIF(CROSS_COMPILE)

```
### 5.3 运行demo

编译完毕后，加入房间并推RTSP视频流。   

```cpp
./enginedemo roomid userid rtsp://xxx

// roomid：自定义的房间号，同一个房间的用户可以通讯。    
// userid：自定义的用户号，每个客户端id需要唯一。    
// rtsp://xxx：通过RTSP拉流设备的网络地址。    

```

推流成功，用户可以处理回调函数`URTCEventHandler.cpp`中的`onLocalPublish`处理，如果code==0，则推流成功。    
推流成功后，即可直播观看RTSP视频。

<!-- tabs:end -->
