

# Windows SDK指南

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


## 5. 初始化

### 5.1 初始化

``` c++
Class UcloudRtcEventListenerImpl ： public UcloudRtcEventListener {
……
};
UcloudRtcEventListener* eventhandler = new UcloudRtcEventListenerImpl

m_rtcengine = UCloudRtcEngine::sharedInstance(eventhandler);
m_rtcengine->setChannelTye(UCLOUD_RTC_CHANNEL_TYPE_COMMUNICATION);
m_rtcengine->setSdkMode(UCLOUD_RTC_SDK_MODE_TRIVAL);
m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
m_rtcengine->setTokenSecKey(TEST_SECKEY);//测试模式下设置自己的秘钥
m_rtcengine->setAudioOnlyMode(false);
m_rtcengine->setAutoPublishSubscribe(false, true);
m_rtcengine->configLocalAudioPublish(false)；
m_rtcengine->configLocalCameraPublish(true);
m_rtcengine->configLocalScreenPublish(false);
tUCloudVideoConfig& videoconfig
m_rtcengine->setVideoProfile(UCLOUD_RTC_VIDEO_PROFILE_640_360，videoconfig); // UCLOUD_RTC_VIDEO_PROFILE_NONE 时 后面填入自定义编码参数  最大1080p(1920*1080)
```

### 5.2 加入房间

``` c++
tUCloudRtcAuth auth;
auth.mAppId = appid;
auth.mRoomId = roomid;
auth.mUserId = userid;
auth.mUserToken = your generate token;
m_rtcengine->joinChannel(auth);
```

### 5.3 发布流

``` c++
tUCloudRtcMediaConfig config;
config.mAudioEnable = true;
config.mVideoEnable = true;
m_rtcengine->publish(UCLOUD_RTC_MEDIATYPE_VIDEO, config.mVideoEnable,config.mAudioEnable);
```

### 5.4 取消发布

``` c++
tUCloudRtcVideoCanvas view;
view.mVideoView = (int)m_localWnd->GetVideoHwnd();
view.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;		
m_rtcengine->stopPreview(view);
m_rtcengine->unPublish(UCLOUD_RTC_MEDIATYPE_VIDEO);
``` 

### 5.5 订阅流
``` c++
m_rtcengine->subscribe(tUCloudRtcStreamInfo & info)
```

### 5.6 取消订阅

``` c++
m_rtcengine->unSubscribe(tUCloudRtcStreamInfo& info)
```

### 5.7 录制视频

#### 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [开通云端录制](https://docs.ucloud.cn/video/urtc/cloudRecord/openRecord)。

```c++
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型
recordconfig.mMainviewuid = m_userid.data(); // 主画面
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD; // 录制等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;
recordconfig.mBucket = "your bucket";
recordconfig.mBucketRegion = "your bucket region";
recordconfig.mIsaverage = false; // 画面是否均分 不均分 均采用 1大几小格式 大画面在左 小画面在右
recordconfig.mWaterMarkType = UCLOUD_RTC_WATERMARK_TYPE_TIME;  // 水印类型
recordconfig.mWatermarkUrl = "hello urtc"; // 如果是文字水印为水印内容   如果是图片则为图片url 地址
recordconfig.mMixerTemplateType = 4; [混流模板](https://docs.ucloud.cn/video/urtc/cloudRecord/RecordLaylout)
m_rtcengine->startRecord(recordconfig);
m_rtcengine->startRecord(recordconfig);

消息回调
//开启录制回调
virtual void onStartRecord (const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均衡模式、垂直模式下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。

更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](https://docs.ucloud.cn/video/urtc/cloudRecord/RecordLaylout)。   

### 5.8 添加背景音（mp3\wav 格式）

```c++
m_rtcengine->startAudioMixing(const char* filepath(本地文件), bool replace（是否取代麦克风输入）, bool loop（是否循环播放）,float musicvol（音乐音量 0.0 -- 1.0）)
``` 

### 5.9 离开房间

``` c++
m_rtcengine->leaveChannel ()
```

### 5.10 编译、运行，开始体验吧！
