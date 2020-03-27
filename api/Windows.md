# Windows

在windows客户端中调用URTC Windows SDK API 可以建立连接，实现实时音视频通话、互动直播服务。      
[点击这里](https://github.com/ucloud/urtc-win-demo)，直接下载GitHub仓库的Windows demo及SDK。    
URTC Windows SDK API包含以下方法：    

| 方法 | 描述 |
| -| -|
|  [UcloudRtcEngine 类](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class)      |  包含URTC的主要方法 | 
|  [UcloudMediaDevice 类](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device)   |  设备引擎接口 |  		
|  [ErrCode](https://github.com/ucloud/urtc-win-demo/tree/master/doc#ErrCode)               |  接口错误表 | 
|  [函数结构体](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct)       	|  函数结构体说明 | 

## 1. UcloudRtcEngine 类包含以下方法：

| 方法 | 描述 |
| -| -|
|  [UCloudRtcEngine](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-UCloudRtcEngine) |  获取引擎 |  
|  [regRtcEventListener](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-regRtcEventListener) |  绑定监听事件 |  
|  [setSdkMode](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setSdkMode) |  设置SDK模式 |  
|  [setChannelType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setChannelType) |  设置通话模式 |  
|  [setStreamRole](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setStreamRole) |  设置流操作权限 |  
|  [setAudioOnlyMode](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setAudioOnlyMode) |  设置纯音频模式 |  
|  [setAutoPublishSubscribe](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setAutoPublishSubscribe) |  设置自动发布和订阅 |  
|  [configLocalAudioPublish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-configLocalAudioPublish) |  配置本地音频是否发布 |  
|  [isLocalAudioPublishEnabled](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-isLocalAudioPublishEnabled) |  是否发布音频 |  
|  [configLocalCameraPublish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-configLocalCameraPublish) |  配置是否自动发布本地摄像头 |  
|  [isLocalCameraPublishEnabled](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-isLocalCameraPublishEnabled) |  是否发布摄像头 |  
|  [configLocalScreenPublish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-configLocalScreenPublish) |  配置是否自动发布本地桌面 |  
|  [isLocalScreenPublishEnabled](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-isLocalScreenPublishEnabled) |  是否发布桌面 |  
|  [setVideoProfile](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setVideoProfile) |  设置视频编码参数 |  
|  [muteCamBeforeJoin](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteCamBeforeJoin) |  入会时关闭摄像头 |  
|  [muteMicBeforeJoin](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteMicBeforeJoin) |  入会时关闭麦克风 |  
|  [joinChannel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-joinChannel) |  加入房间 |  
|  [leaveChannel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-leaveChannel) |  离开房间 |  
|  [publish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-publish) |  发布本地流 |  
|  [unPublish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-unPublish) |  停止发布本地流 |  
|  [startPreview](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-startPreview) |  开启本地渲染 |  
|  [stopPreview](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-stopPreview) |  停止本地渲染 |  
|  [subscribe](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-subscribe) |  订阅远端媒体流 |  
|  [unSubscribe](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-unSubscribe) |  取消订阅远端媒体流 |  
|  [startRemoteView](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-startRemoteView) |  开启远端渲染 |  
|  [stopRemoteView](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-stopRemoteView) |  停止远端渲染 |  
|  [muteLocalMic](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteLocalMic) |  打开/关闭本地麦克风 |  
|  [muteLocalVideo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteLocalVideo) |  打开/关闭本地视频 |  
|  [enableAllAudioPlay](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-enableAllAudioPlay) |  应用静音 |  
|  [muteRemoteAudio](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteRemoteAudio) |  打开/关闭远端音频 |  
|  [muteRemoteVideo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-muteRemoteVideo) |  打开/关闭远端视频 |  
|  [isAutoPublish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-isAutoPublish) |  是否为自动发布模式 |  
|  [isAutoSubscribe](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-isAutoSubscribe) |  是否为自动订阅模式 |  
|  [switchCamera](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-switchCamera) |  切换摄像头 |  
|  [enableExtendRtspVideocapture](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-enableExtendRtspVideocapture) |  设置RTSP视频源 |  
|  [enableExtendVideocapture](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-enableExtendVideocapture) |  设置自定义外部视频源 |  
|  [regAudioFrameCallback](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-regAudioFrameCallback) |  设置音频数据监听 |  
|  [startAudioMixing](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-startAudioMixing) |  添加micphone混音文件 |  
|  [stopAudioMixing](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-stopAudioMixing) |  停止micphone混音 |  
|  [setDesktopProfile](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setDesktopProfile) |  设置桌面编码参数 |  
|  [setCaptureScreenPagrams](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setCaptureScreenPagrams) |  设置桌面采集参数 |  
|  [setUseDesktopCapture](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setUseDesktopCapture) |  设置桌面采集类型 |  
|  [getDesktopNums](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-getDesktopNums) |  获取屏幕个数 |  
|  [getDesktopInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-getDesktopInfo) |  获取屏幕信息 |  
|  [getWindowNums](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-getWindowNums) |  获取窗口个数 |  
|  [getWindowInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-getWindowInfo) |  获取窗口信息 |  
|  [startRecord](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-startRecord) |  开启录制 |  
|  [stopRecord](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-stopRecord) |  停止录制 |  
|  [setLogLevel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-setLogLevel) |  设置日志等级 |  
|  [getSdkVersion](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-getSdkVersion) |  获取SDK版本 |  
|  [destroy](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-destroy) |  销毁引擎 |  
|  [enableExtendAudiocapture](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-enableExtendAudiocapture) |  设置外部音频采集 |  
|  [regDeviceChangeCallback](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-regDeviceChangeCallback) |  注册设备热插拔回调通知 |  
|  [addPublishStreamUrl](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-addPublishStreamUrl) |  旁路推流 |  
|  [removePublishStreamUrl](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-removePublishStreamUrl) |  停止旁路推流 |  
|  [updateRtmpMixStream](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-updateRtmpMixStream) |  更新旁路推流合流的流 |  

## 2. UcloudMediaDevice设备引擎接口类

| 方法 | 描述 |
| -| -|
| [UCloudRtcMediaDevice](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-UCloudRtcMediaDevice) | 初始化设备模块 | 
| [destory](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-destory) | 销毁设备模块 | 
| [InitVideoMoudle](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-InitVideoMoudle) | 初始化视频模块 | 
| [UnInitVideoMoudle](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-UnInitVideoMoudle) | 销毁视频模块 | 
| [InitAudioMoudle](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-InitAudioMoudle) | 初始化音频模块 | 
| [UnInitAudioMoudle](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-UnInitAudioMoudle) | 销毁音频模块 | 
| [getCamNums](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getCamNums) | 获取摄像头数量 | 
| [getRecordDevNums](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getRecordDevNums) | 获取麦克风数量 | 
| [getPlayoutDevNums](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getPlayoutDevNums) | 获取播放设备数量 | 
| [getVideoDevInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getVideoDevInfo) | 获取摄像头设备信息 | 
| [getRecordDevInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getRecordDevInfo) | 获取麦克风设备信息 | 
| [getPlayoutDevInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getPlayoutDevInfo) | 获取播放设备信息 | 
| [getCurCamDev](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getCurCamDev) | 获取当前使用的摄像头信息 | 
| [getCurRecordDev](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getCurRecordDev) | 获取当前使用的麦克风设备信息 | 
| [getCurPlayoutDev](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getCurPlayoutDev) | 获取当前使用的播放设备信息 | 
| [setVideoDevice](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-setVideoDevice) | 设置视频设备 | 
| [setRecordDevice](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-setRecordDevice) | 设置麦克风设备 | 
| [setPlayoutDevice](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-setPlayoutDevice) | 设置播放设备 | 
| [getPlaybackDeviceVolume](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getPlaybackDeviceVolume) | 获取应用播放音量 | 
| [setPlaybackDeviceVolume](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-setPlaybackDeviceVolume) | 设置应用播放音量 | 
| [getRecordingDeviceVolume](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-getRecordingDeviceVolume) | 获取系统麦克风音量 | 
| [setRecordingDeviceVolume](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-setRecordingDeviceVolume) | 设置系统麦克风音量 | 
| [startCamTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-startCamTest) | 开始摄像头测试 | 
| [stopCamTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-stopCamTest) | 停止摄像头测试 | 
| [startRecordingDeviceTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-startRecordingDeviceTest) | 开始麦克风测试 | 
| [stopRecordingDeviceTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-stopRecordingDeviceTest) | 停止麦克风测试 | 
| [startPlaybackDeviceTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-startPlaybackDeviceTest) | 开始播放设备测试 | 
| [stopPlaybackDeviceTest](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-stopPlaybackDeviceTest) | 停止播放设备测试 | 
| [startCaptureFrame](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-startCaptureFrame) | 开始采集视频数据回调 | 
| [stopCaptureFrame](https://github.com/ucloud/urtc-win-demo/tree/master/doc#Device-stopCaptureFrame) | 停止采集视频数据回调 | 

## 3. ErrCode接口错误表

<a name='ErrCode-shijian'></a>

###  3.1  事件回调错误码

```cpp
typedef enum _tUCloudRtcCallbackErrCode{    
	UCLOUD_RTC_CALLBACK_ERR_CODE_OK = 0,  //成功    
	UCLOUD_RTC_CALLBACK_ERR_SERVER_CON_FAIL = 5000, //服务器连接失败    
	UCLOUD_RTC_CALLBACK_ERR_SERVER_DIS, // 服务端连接断开    
	UCLOUD_RTC_CALLBACK_ERR_SAME_CMD,  //重复的调用    
	UCLOUD_RTC_CALLBACK_ERR_NOT_IN_ROOM, //未加入房间 无发进行操作     
	UCLOUD_RTC_CALLBACK_ERR_ROOM_JOINED, // 已加入房间 无需加入    
	UCLOUD_RTC_CALLBACK_ERR_SDK_INNER,   // SDK 内部错误    
	UCLOUD_RTC_CALLBACK_ERR_ROOM_RECONNECTING, // 重连中 请求无法投递          
	UCLOUD_RTC_CALLBACK_ERR_STREAM_PUBED,  // 流已经发布  无需发布    
	UCLOUD_RTC_CALLBACK_ERR_PUB_NO_DEV,  // 发布无可用 音频 视频设备    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_NOT_PUB, //流没有发布 无法对流进行操作    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_SUBED,  //流已经订阅  无需订阅    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_NO_SUB, //流没有订阅  无法    
	UCLOUD_RTC_CALLBACK_ERR_SUB_NO_USER,   //无对应的用户 无法订阅    
	UCLOUD_RTC_CALLBACK_ERR_SUB_NO_STREAM,  // 无对应的媒体流    
	UCLOUD_RTC_CALLBACK_ERR_USER_LEAVING, // 用户正在离开房间  无法进行其他操作    
	UCLOUD_RTC_CALLBACK_ERR_NO_HAS_TRACK,  //无对应的媒体轨道    
	UCLOUD_RTC_CALLBACK_ERR_MSG_TIMEOUT, // 消息请求超时    
}tUCloudRtcCallbackErrCode;    
```

<a name='ErrCode-hanshu'></a>

###  3.2 函数值错误码

```cpp
typedef enum _tUCloudRtcReturnErrCode {    
	UCLOUD_RTC_RETURN_ERR_CODE_OK = 0,  //成功    
	UCLOUD_RTC_RETURN_ERR_AUTO_PUB = 1000, //自动发布    
	UCLOUD_RTC_RETURN_ERR_AUTO_SUB, //自动订阅    
	UCLOUD_RTC_RETURN_ERR_NOT_INIT, //引擎没有初始化    
	UCLOUD_RTC_RETURN_ERR_IN_ROOM, //已经加入房间    
	UCLOUD_RTC_RETURN_ERR_NOT_IN_ROOM, // 未加入房间    
	UCLOUD_RTC_RETURN_ERR_NOT_PUB_TRACK, //未发布对应媒体    
	UCLOUD_RTC_RETURN_ERR_INVAILED_PARGRAM, // 无效参数    
	UCLOUD_RTC_RETURN_ERR_INVAILED_WND_HANDLE, // 无效窗口句柄    
	UCLOUD_RTC_RETURN_ERR_INVAILED_MEDIA_TYPE, // 无效媒体类型    
	UCLOUD_RTC_RETURN_ERR_SUB_ONEMORE, // 最少订阅一种媒体    
	UCLOUD_RTC_RETURN_ERR_NO_PUB_ROLE, //无发布权限    
	UCLOUD_RTC_RETURN_ERR_NO_SUB_ROLE, //无订阅权限    
	UCLOUD_RTC_RETURN_ERR_CAM_NOT_ENABLE, //没有配置本地cam 发送    
	UCLOUD_RTC_RETURN_ERR_SCREEN_NOT_ENABLE, //没有配置本地screen 发送    
	UCLOUD_RTC_RETURN_ERR_AUDIO_MODE,        // 纯音频模式    
	UCLOUD_RTC_RETURN_ERR_SECKEY_INVALID ,    // seckey 无效    
	UCLOUD_RTC_RETURN_ERR_INVAILD_FILEPATH,    
	UCLOUD_RTC_RETURN_ERR_NOT_SUPORT_AUDIO_FORMAT,    
}tUCloudRtcReturnErrCode;    
```
## 4. 函数结构体说明

| 方法 | 描述 |
| -| -|
| [tUCloudRtcDeviceInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcDeviceInfo) | 设备信息类 | 
| [tUCloudRtcMediaConfig](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcMediaConfig) | 媒体发布配置类 | 
| [eUCloudRtcTrackType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcTrackType) | 媒体轨道类型类型描述 | 
| [eUCloudRtcMeidaType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcMeidaType) | 媒体流类型描述 | 
| [tUCloudRtcStreamInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcStreamInfo) | 流信息结构体 | 
| [eUCloudRtcWaterMarkPos](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcWaterMarkPos) | 录制水印位置 | 
| [eUCloudRtcWaterMarkType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcWaterMarkType) | 水印类型 | 
| [tUCloudRtcMuteSt](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcMuteSt) | Mute操作结构体 | 
| [eUCloudRtcRecordProfile](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcRecordProfile) | 录制输出等级 | 
| [eUCloudRtcRecordType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcRecordType) | 录制媒体类型 | 
| [tUCloudRtcRecordConfig](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcRecordConfig) | 录制配置信息 | 
| [eUCloudRtcRenderMode](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcRenderMode) | 渲染模式 | 
| [eUCloudRtcLogLevel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcLogLevel) | 日志级别 | 
| [eUCloudRtcVideoProfile](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcVideoProfile) | 视频质量参数 | 
| [eUCloudRtcScreenProfile](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcScreenProfile) | 桌面输出参数 | 
| [tUCloudRtcScreenPargram](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcScreenPargram) | 桌面采集参数 | 
| [eUCloudRtcDesktopType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcDesktopType) | 桌面采集类型 | 
| [tUCloudRtcDeskTopInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcDeskTopInfo) | 桌面参数 | 
| [eUCloudRtcUserStreamRoleCHANNEL](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcUserStreamRoleCHANNEL) | 通道类型 | 
| [eUCloudRtcUserStreamRoleSTREAM](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcUserStreamRoleSTREAM) | 流权限 | 
| [tUCloudRtcVideoCanvas](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcVideoCanvas) | 渲染窗口 | 
| [tUCloudRtcAuth](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcAuth) | 登录信息类 | 
| [tUCloudRtcStreamStats](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcStreamStats) | 当前媒体状态统计 | 
| [tUCloudRtcRecordInfo](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcRecordInfo) | 录制信息回调 | 
| [tUCloudRtcAudioFrame](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcAudioFrame) | 音频帧 | 
| [eUCloudRtcVideoFrameType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcVideoFrameType) | 视频数据帧类型 | 
| [tUCloudRtcVideoFrame](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudRtcVideoFrame) | 视频数据帧 | 
| [UCloudRtcEventListener](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcEventListener) | 消息回调事件接口类 | 
| [UCloudRtcMediaListener](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcMediaListener) | 音频测试回调 | 
| [UCloudRtcAudioFrameCallback](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcAudioFrameCallback) | 音频数据回调 | 
| [UCloudRtcExtendVideoCaptureSource](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcExtendVideoCaptureSource) | 视频扩展数据源 | 
| [UCloudRtcExtendVideoRender](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcExtendVideoRender) | 视频数据回调 | 
| [UCloudRtcVideoFrameObserver](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UCloudRtcVideoFrameObserver) | 视频数据回调监听类（yuv420p格式） | 
| [eUCloudRtcRenderType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcRenderType) | 视频渲染类型 | 
| [eUCloudRtcVideoCodec](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcVideoCodec) | 视频编码类型 | 
| [tUCloudVideoConfig](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-tUCloudVideoConfig) | 视频参数 | 
| [eUCloudRtcNetworkQuality](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcNetworkQuality) | 上下行网络类型 | 
| [eUCloudRtcQualityType](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-eUCloudRtcQualityType) | 网络评分 | 
| [UcloudRtcDeviceChanged](https://github.com/ucloud/urtc-win-demo/tree/master/doc#struct-UcloudRtcDeviceChanged) | 热插拔回调 | 

