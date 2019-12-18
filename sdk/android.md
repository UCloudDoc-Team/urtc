

# Android SDK指南

## 1. 下载资源

  - 可以下载Demo、SDK、API Java 文档  
    [现在下载](https://github.com/ucloud/urtc-android-demo)

## 2. 开发语言以及系统要求

  - 开发语言：Java
  - 系统要求：Android 5.0以上版本

## 3. 开发环境

  - android studio 需要android SDK

## 4. 搭建开发环境

  - 下载urtc android SDK包，SDK包为aar
    格式，名称为`ucloudrtclib`开头加版本号加一串8位识别码，可以参考github上的接入demo。
  - 将aar 文件拷贝到自己的`lib` 目录下，然后添加到`lib` 中，修改要使用sdk模块目录下`build.gradle`，确保已经添加了如下依赖，如下所示：



    dependencies {
    implementation (name: 'ucloudrtclib_1.0.1_b52bc04c', ext: 'aar')

  - 如果项目混淆，请在混淆中添加一下urtc 混淆规则



```js
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
在实时音视频SDK 中，用户需要在进入音视频通话房间前动态申请 `CAMERA`、`RECORD\_AUDIO`、`WRITE\_EXTERNAL\_STORAGE`权限，具体可以参考[Android官方文档](https://developer.android.com/training/permissions/requesting?hl=zh-cn)

```js
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

主要配置`android context sdkmode`以及`AppID` ，测试用的`SEC\_KEY`，日志等级

```js
public class UCloudRtcApplication extends Application {
        @Override
        public void onCreate() {
            super.onCreate();
            URTCSdkEnv.initEnv(getApplicationContext(), this);
            URTCSdkEnv.setLogLevel(URTCSdkLogLevel.URTC_SDK_LogLevelInfo) ;
            URTCSdkEnv.setSdkMode(URTCSdkMode.RTC_SDK_MODE_TRIVAL);
            URTCSdkEnv.setTokenSeckey(CommonUtils.SEC_KEY);
            WindowManager windowManager = (WindowManager) 
            getSystemService(Context.WINDOW_SERVICE);
            DisplayMetrics outMetrics = new DisplayMetrics();
            windowManager.getDefaultDisplay().getMetrics(outMetrics);
            CommonUtils.mItemWidth = outMetrics.widthPixels / 3;
            CommonUtils.mItemHeight = CommonUtils.mItemWidth;
        }
    }
```

### 5.2 继承实现`UCloudRtcSdkEventListener` 实现事件处理


```js
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

```js
sdkEngine.setAudioOnlyMode(true) ; // 设置纯音频模式
sdkEngine.configLocalCameraPublish(false) ; // 设置摄像头是否发布
sdkEngine.configLocalAudioPublish(true) ; // 设置音频是否发布，用于让sdk判断自动发布的媒体类型
sdkEngine.configLocalScreenPublish(false) ; // 设置桌面是否发布，作用同上
sdkEngine.setStreamRole(URTCSdkStreamRole.URTC_SDK_STREAM_ROLE_BOTH);// 流权限
sdkEngine.setAutoPublish(true) ; // 是否自动发布
sdkEngine.setAutoSubscribe(true) ;// 是否自动订阅
sdkEngine.setVideoProfile(UCloudRtcSdkVideoProfile.matchValue(mVideoProfile)) ;// 摄像头输出等级
```

## 6. 建立通话

### 6.1 加入房间



```js
UCloudRtcSdkAuthInfo info = new UCloudRtcSdkAuthInfo();
        info.setAppId(mAppid);
        info.setToken(mRoomToken);
        info.setRoomId(mRoomid);
        info.setUId(mUserid);
        Log.d(TAG, " roomtoken = " + mRoomToken);
        sdkEngine.joinChannel(info); 
```

### 6.2 设置自动/手动发布音视频流

  - 如果配置了自动发布无需调用发布视频接口，SDK会在用户成功加入房间后自动发布，只需要监听事件调用渲染接口即可。
  - 如果配置了手动发布需要调用`sdkEngine`引擎的`publish`接口 配置手动/自动发布。

```js
sdkEngine.setAutoPublish(mPublishMode == CommonUtils.AUTO_MODE ? true : false);
```

  - 媒体发布类型

现在的类型包括两大类，需要传入`publish`接口的`mtype`,`hasvideo`,`hasaudio`参数各不相同，混合类型是单一类型的组合，具体代码可参阅urtcdemo的`RoomActvity`中的处理。

  - 混合类型

 * 音频+屏幕捕捉
 * 视频+屏幕捕捉
- 单一类型
 * 音频 （mtype:urtc_sdk_media_type_video,hasvideo:false,hasaudio:true）
 * 视频 （mtype:urtc_sdk_media_type_video,hasvideo:true,hasaudio:true）
 * 屏幕捕捉 （mtype:urtc_sdk_media_type_screen,hasvideo:true,hasaudio:false）

  - 手动发布媒体流


```js
sdkEngine.publish(UCloudRtcSdkMediaType mtype, boolean hasvideo, boolean hasaudio)
回调事件
public void onLocalPublish(int code, String msg, UCloudRtcSdkStreamInfo info
```

### 6.3 渲染媒体流

在`onLocalPublish` 回调成功后，再函数中可以调用视频渲染。

```js
localrenderview.setBackgroundColor(Color.TRANSPARENT);
sdkEngine.startPreview(info.getmMediatype(), localrenderview);
不想渲染时可以调用停止渲染接口
sdkEngine.stopPreview(UCloudRtcSdkMediaType mediatype
```

  - 取消发布媒体流

```js
sdkEngine.unPublish(UCloudRtcSdkMediaType mtype)
回调事件
public void onLocalUnPublish(int code, String msg, UCloudRtcSdkStreamInfo info
```

### 6.4 订阅媒体流

如果配置了自动订阅无需调用订阅视频接口，SDK会在用户成功加入房间后查看房间已有的可以订阅的流并进行逐一订阅，当有新用户加入房间时也会自动订阅他推的流。   
如果配置了手动订阅需要调用sdkEngine引擎的subscribe接口。 

```js
sdkEngine.setAutoSubscribe(mScribeMode == CommonUtils.AUTO_MODE ? true : false);
```

  - 订阅媒体流

```js
sdkEngine.subscribe(UCloudRtcSdkStreamInfo info)
//回调事件
public void onSubscribeResult(int code, String msg, UCloudRtcSdkStreamInfo info
```

  - 渲染媒体流

在onSubscribeResult回调成功后，再函数中可以调用视频渲染

```js
sdkEngine. startRemoteView(UCloudRtcSdkStreamInfo info, UCloudRtcSdkSurfaceVideoView renderview)
//不想渲染时可以调用定制渲染接口
sdkEngine.stopPreview(UCloudRtcSdkMediaType mediatype
```

  - 取消订阅媒体流

```js
sdkEngine. subscribe(UCloudRtcSdkStreamInfo info) 
//回调事件
public void onUnSubscribeResult(int code, String msg, UCloudRtcSdkStreamInfo info)
```

### 6.4 用户发布和订阅的权限控制

权限分为发布，订阅，全部权限，全部权限包括了发布和订阅。

```js
//接口
public int setStreamRole(UCloudRtcSdkStreamRole role)
//调用
sdkEngine.setStreamRole(mRole);
```
   
### 6.5 录像

录像目前只支持摄像头录制，不支持桌面录制，`region`和`bucket`这两个参数默认用了ucloud的`region`和`bucket`，如果用自己的需要上ucloud控制台申请自己的录像存储空间，服务器会通过UCloudRtcSdkEventListener 的onRecordStart()接口作为回调返回录像开始结果。

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均衡模式、平铺模式下，主界面是哪个用户，哪个用户就占据大窗口。同时，这里的用户可以是当前App中推流的用户，也可以是当前App中被订阅的用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](https://docs.ucloud.cn/video/urtc/cloudRecord/RecordLaylout)。   

```js
//                如果主窗口是当前用户
UcloudRtcSdkRecordProfile recordProfile = UcloudRtcSdkRecordProfile.getInstance().assembleRecordBuilder()
                        .recordType(UcloudRtcSdkRecordProfile.RECORD_TYPE_VIDEO)
                        .mainViewMediaType(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
                        .VideoProfile(UCloudRtcSdkVideoProfile.UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480.ordinal())
                        .Average(UcloudRtcSdkRecordProfile.RECORD_UNEVEN)
                        .WaterType(UcloudRtcSdkRecordProfile.RECORD_WATER_TYPE_IMG)
                        .WaterPosition(UcloudRtcSdkRecordProfile.RECORD_WATER_POS_LEFTTOP)
                        .WarterUrl("http://urtc-living-test.cn-bj.ufileos.com/test.png")
                        .Template(UcloudRtcSdkRecordProfile.RECORD_TEMPLET_9)
                        .build();
                sdkEngine.startRecord(recordProfile);
                //如果主窗口不是当前推流用户，而是被订阅的用户
//                UCloudRtcSdkStreamInfo uCloudRtcSdkStreamInfo = mVideoAdapter.getStreamInfo(0);
//                if(uCloudRtcSdkStreamInfo != null){
//                    UcloudRtcSdkRecordProfile recordProfile = UcloudRtcSdkRecordProfile.getInstance().assembleRecordBuilder()
//                            .recordType(UcloudRtcSdkRecordProfile.RECORD_TYPE_VIDEO)
//                            .mainViewUserId(uCloudRtcSdkStreamInfo.getUId())
//                            .mainViewMediaType(uCloudRtcSdkStreamInfo.getMediaType().ordinal())
//                            .VideoProfile(UCloudRtcSdkVideoProfile.UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480.ordinal())
//                            .Average(UcloudRtcSdkRecordProfile.RECORD_UNEVEN)
//                            .WaterType(UcloudRtcSdkRecordProfile.RECORD_WATER_TYPE_IMG)
//                            .WaterPosition(UcloudRtcSdkRecordProfile.RECORD_WATER_POS_LEFTTOP)
//                            .WarterUrl("http://urtc-living-test.cn-bj.ufileos.com/test.png")
//                            .Template(UcloudRtcSdkRecordProfile.RECORD_TEMPLET_9)
//                            .build();
//                    sdkEngine.startRecord(recordProfile);
//                }

//UCloudRtcSdkEventListener 
//录像开始回调
void onRecordStart(int code,String fileName);

//录像结束
sdkEngine.stopRecord();

//UCloudRtcSdkEventListener 结束回调
void onRecordStop(int code);
```  
  
### 6.6 外部扩展输入与输出
sdk支持`rgba`系列数据（`rgba`，`abgr`，`rgb565`）以及`yuv420p`的外部自定义输入，能够产出拉流的`rgba`,`abgr`的数据供使用者自行扩展使用，具体使用方式请参考 [demo](https://github.com/ucloud/urtc-android-demo)路径下的`rgb转yuv接口使用说明.md` 和 `yuv转rgb接口使用说明.md`。


### 6.7 离开房间


```js
sdkEngine.leaveChannel() ;
```

### 6.8 编译、运行，开始体验吧！
