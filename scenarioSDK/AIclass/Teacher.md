# 教师端 SDK指南

## 1. 功能说明

  - 推mp4视频流
  - 无缝切换视频源
  - 接收学生推流，后续对接AI处理语音和视频

## 2. 下载资源

  - 可以下载Demo、SDK、API文档  
  
    [下载Demo](https://github.com/ucloud/urtc-linux-demo)      
  
    [下载SDK和API文档](http://urtcsdk.cn-bj.ufileos.com/urtclib.zip)      

## 3. 开发语言以及系统要求

  - 开发语言：C++  
  - 系统要求：x64 ubuntu14.04 及以上

## 4. 开发环境

 gcc4.9.4

## 5. 搭建开发环境

  - 导入 SDK    
  
1） 将 urtclib/include 目录添加到项目的 INCLUDE 目录下。    
2） 将 urtclib/lib 目录放入项目的 LIB 目录下。  
3)  编写cmake 或者 makefile 编译 


## 6. 快速接入 
### 6.1. 继承实现UCloudRtcEventListener，用作事件处理

```c++
Class UcloudRtcEventListenerImpl ： public UcloudRtcEventListener {
……
};
UcloudRtcEventListener* eventhandler = new UcloudRtcEventListenerImpl
```

### 6.2 初始化引擎 

``` c++
m_rtcengine = UCloudRtcEngine::sharedInstance(eventhandler);
m_rtcengine = UCloudRtcEngine::sharedInstance(UCloudRtcEventListener实现类);
m_rtcengine = UCloudRtcEngine::sharedInstance(eventhandler);
m_rtcengine->setSdkMode(UCLOUD_RTC_SDK_MODE_TRIVAL);
m_rtcengine->setChannelTye(UCLOUD_RTC_CHANNEL_TYPE_COMMUNICATION);
m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
m_rtcengine->setTokenSecKey(TEST_SECKEY);//测试模式下设置自己的秘钥
m_rtcengine->setAudioOnlyMode(false);
m_rtcengine->setAutoPublishSubscribe(false, true);
m_rtcengine->configLocalAudioPublish(false)；
m_rtcengine->configLocalCameraPublish(true);
m_rtcengine->configLocalScreenPublish(false);
```

### 6.3 添加mp4 列表

``` c++
m_rtcengine->addMp4File(filebuf, size, cleanup);
```

### 6.4 加入房间

```c++
tUCloudRtcAuth auth;
auth.mAppId = appid;
auth.mRoomId = roomid;
auth.mUserId = userid;
auth.mUserToken = "1223222";
m_rtcengine->joinChannel(auth);
```

### 6.5 发布本地文件列表

```c++
tUCloudRtcMediaConfig config;
config.mAudioEnable = true;
config.mVideoEnable = true;
m_rtcengine->publish(UCLOUD_RTC_MEDIATYPE_VIDEO, config.mVideoEnable,
            config.mAudioEnable)
```

### 6.6 取消发布文件列表

```c++
tUCloudRtcVideoCanvas view;
view.mVideoView = (int)m_localWnd->GetVideoHwnd();
view.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;
m_rtcengine->stopPreview(view);
m_rtcengine->unPublish(UCLOUD_RTC_MEDIATYPE_VIDEO);
```

### 6.7 订阅流

```c++
m_rtcengine->subscribe(tUCloudRtcStreamInfo & info)
```

### 6.8 取消订阅流

```c++
m_rtcengine->unSubscribe(tUCloudRtcStreamInfo& info)
```


### 6.9 离开房间

```c++
m_rtcengine->leaveChannel ()
```
