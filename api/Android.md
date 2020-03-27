# URTC Android SDK API指南

在Android客户端中调用URTC Android SDK API 可以建立连接，实现实时音视频通话、互动直播服务。      
[点击这里](https://github.com/ucloud/urtc-android-demo)，直接下载GitHub仓库的Android demo、SDK及API文档。   
    
URTC Android SDK API包含以下方法：    

| 方法 | 描述 |
| -| -|
| UCloudRtcSdkEngine  | 包含URTC的主要方法  |
| UCloudRtcSdkSurfaceVideoView.RemoteOpTrigger  | 静音远端音视频  |
| UcloudRTCSceenShot  | 截屏  |
| UcloudRTCDataProvider  | 外部数据推流  |

## 1. UCloudRtcSdkEngine类包含以下方法

| 方法 | 描述 |
| -| -|
| createEngine  | 获取引擎类  |
| destory  | 销毁引擎  |
| getDefaultAudioDevice  | 获取音频默认设备  |
| getNativeOpInterface  | 获取底层接口  |
| getSdkVersion  | 获取sdk版本  |
| configLocalAudioPublish   | 配置本地音频是否发布  |
| configLocalCameraPublish  | 配置本地摄像头是否发布  |
| configLocalScreenPublish  | 配置本地桌面是否发布  |
| isAudioOnlyMode  | 是否为纯音频模式  |
| isAutoPublish    | 是否为自动发布  |
| isAutoSubscribe  | 是否为自动订阅  |
| isLocalAudioPublishEnabled   | 音频是否发布   |
| isLocalCameraPublishEnabled  | 摄像头是否发布  |
| isLocalScreenPublishEnabled  | 桌面是否发布  |
| joinChannel  | 加入房间  |
| leaveChannel  | 离开房间  |
| setClassType  | 设置房间类型  |
| setStreamRole  | 设置流操作（全部、发布、订阅）权限  |
| muteLocalMic  | 打开关闭本地音频（音频静音包发送）  |
| muteLocalVideo  | 打开关闭本地视频（黑屏帧发送）  |
| muteRemoteAudio  | 静音开启远端音频  |
| muteRemoteScreen  | 打开关闭远端桌面  |
| muteRemoteVideo  | 打开关闭远端视频  |
| onScreenCaptureResult  | 获取桌面采集权限结果  |
| publish  | 发布媒体流  |
| unPublish  | 停止媒体流发布  |
| lockExtendDeviceInputBuffer  | 锁定外部扩展设备输入缓存,用于和轮询线程同步  |
| releaseExtendDeviceInputBuffer  | 解锁外部扩展设备输入缓存,用于和轮询线程同步  |
| requestScreenCapture  | 获取桌面采集权限  |
| setAudioDevice  | 设置音频设备  |
| switchCamera  | 切换摄像头  |
| setAudioOnlyMode  | 设置纯音频模式  |
| setAutoPublish  | 设置自动发布或者手动发布  |
| setAutoSubscribe  | 设置自动订阅手动订阅  |
| setSpeakerOn  | 设置扬声器状态  |
| setVideoProfile  | 设置视频分辨率  |
| startPreview  | 开启本地预览  |
| stopPreview  | 停止本地预览  |
| startRemoteView  | 开启远端渲染 函数只在自己订阅成功后才能调用  |
| stopRemoteView  | 停止远端渲染  |
| subscribe  | 订阅远端媒体流  |
| unSubscribe  | 取消订阅远端媒体流  |
| startRecord  | 开始录制  |
| stopRecord  | 结束录制  |
| SCREEN_CAPTURE_REQUEST_CODE  | 申请桌面权限请求码  |
| UCloudRtcSdkEventListener  | Sdk 事件回调  |

## 2. UCloudRtcSdkSurfaceVideoView.RemoteOpTrigger接口

| 方法 | 描述 |
| -| -|
| onRemoteAudio  | 静音远端音频  |
| onRemoteVideo  | 静音远端视频  |

## 3. UcloudRTCSceenShot接口

| 方法 | 描述 |
| -| -|
| onReceiveRGBAData  | 输出的截屏数据，rgba格式   |

## 4. UcloudRTCDataProvider接口

| 方法 | 描述 |
| -| -|
| provideRGBData  | 供sdk采集外部数据进行推流的方法  |
| releaseBuffer  | 释放申请的buffer  |
