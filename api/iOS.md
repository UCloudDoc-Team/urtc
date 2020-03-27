# URTC iOS SDK API指南

在iOS客户端中调用URTC iOS SDK API 可以建立连接，实现实时音视频通话、互动直播服务。     
[点击这里](https://github.com/ucloud/urtc-ios-demo)，直接下载GitHub仓库的iOS demo、SDK及API文档。   
  
URTC iOS SDK API包含以下方法：    

| 方法 | 描述 |
| -| -|
| UCloudRtcEngine  | 包含URTC的主要方法  |
| UCloudRtcStream  | 指定视频渲染  |
| UCloudRtcLog  | 调试时打印内部日志  |
| UCloudRtcEngineDelegate  | 监听SDK各种状态  |

## 1. UCloudRtcEngine类包含以下方法

| 方法 | 描述 |
| -| -|
| engineMode  | SDK模式 默认测试，正式上线前需要切换到正式模式  |
| isAutoPublish  | 是否自动发布 默认是  |
| isAutoSubscribe  | 是否自动订阅 默认是  |
| isOnlyAudio  | 是否开启纯音频模式 默认否  |
| videoProfile  | 设置视频分辨率 默认:480*360  |
| roomType  | 设置房间类型 默认是会议模式（小班课）  |
| streamProfile  | 设置流操作权限 默认:全部权限  |
| isLoudSpeaker  | 是否开启免提  |
| localStream  | 本地流  |
| fileMix  | 是否开启混音  |
| filePath   | 混音文件路径  |
| fileLoop  | 混音文件是否循环播放  |
| videoDefaultCodec  | 视频编码格式 默认VP8 可选 VP8、H264  |
| videopath  | 本地录制时视频文件路径  |
| audiopath  | 本地录制时音频文件路径  |
| outputpath  | 本地录制时文件输出路径  |
| delegate  | 引擎的代理  |
| initWithUserId:appId:roomId:appKey:token  | 初始化UCloudRtcEngine  |
| joinRoomWithcompletionHandler  | 加入房间  |
| leaveRoom  | 退出房间  |
| publish  | 发布  |
| unPublish  | 取消发布  |
| subscribeMethod | 手动订阅  |
| unSubscribeMethod  | 取消订阅  |
| setLocalPreview  | 设置本地的预览画面  |
| switchCamera  | 切换本地摄像头  |
| setMute  | 设置本地流是否静音  |
| openCamera  |  设置本地流是否禁用视频 |
| openLoudspeaker  | 开启免提  |
| setRemoteStream:muteVideo  | 设置远程流是否禁用视频  |
| setRemoteStream:muteAudio  | 设置远程流是否禁用音频  |
| startRecord  | 开始录制  |
| stopRecord  | 停止录制  |
| startNativeRecord  | 开始本地录制  |
| stopNativeRecord  | 停止本地录制  |
| startMediaPlay:repeat  | 播放网络音频  |
| stopMediaPlay  | 停止播放网络音频  |
| sendCustomCommand  | 发送自定义消息  |
| currentVersion  | 返回SDK当前版本号  |


## 2. UCloudRtcStream接口

| 方法 | 描述 |
| -| -|
| renderOnView  | 渲染到指定视图  |
| getReportStates  | 获取流状态信息  |



## 3. UCloudRtcLog接口

| 方法 | 描述 |
| -| -|
| setLogLevel  | 设置SDK日志级别  |
| setRollBack  | 设置日志回滚大小 |
| logup:withLogType  | 日志埋点上报  |

## 4. UCloudRtcEngineDelegate接口

| 方法 | 描述 |
| -| -|
| uCloudRtcEngine:roomState:streamList  | 监听房间状态变化  |
| uCloudRtcEngine:didChangePublishState  | 监听流发布状态的变化  |
| uCloudRtcEngine:remoteStreamState:remoteStream  | 监听远程流状态变化  |
| uCloudRtcEngine:roomMemberState:memberInfo  | 监听房间内成员变化 |
| uCloudRtcEngine:subscribeStreamState:subscribeStream  | 在非自动订阅模式下，监听可订阅流的状态  |
| uCloudRtcEngine:didReceiveStreamStatus  | 流的状态回调  |
| uCloudRtcEngine:streamConnectionFailed  | 流连接失败回调  |
| uCloudRtcEngine:error  | SDK错误回调  |
| uCloudRtcEngine:startRecord   | 开始录制视频的回调  |
| uCloudRtcEngine:receiveCustomCommand:content  | 收到自定义消息的回调  |


