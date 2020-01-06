# SDK 版本说明

## 1.6.1 版本发布

* 1、加入大班课功能，支持超大规模会议功能
使用如下
``` c++ 
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```
* 2、添加自定义编码（最大分辨率到1080p(1920*1080)）
``` c++ 
virtual void setVideoProfile(eUCloudRtcVideoProfile profile) = 0;
变更为
virtual void setVideoProfile(eUCloudRtcVideoProfile profile, tUCloudVideoConfig& videoconfig) = 0;
tUCloudVideoConfig 用来定义自己编码分辨率
```
* 3、录制功能增加更多录制模板以及水印 调用示例如下
``` c++ 
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型 video screen
recordconfig.mMainviewuid = m_userid.data(); // 主画面用户id
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD;//录制输出等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;//录制媒体内容
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;//水印位置
recordconfig.mBucket = "urtc-test";  // ufile bucket
recordconfig.mBucketRegion = "cn-bj"; // ufile region
recordconfig.mIsaverage = false; // 画面是否均分 不均分 均采用 1大几小格式 大画面在左 小画面在右
recordconfig.mWaterMarkType = UCLOUD_RTC_WATERMARK_TYPE_TIME;  // 水印类型
recordconfig.mWatermarkUrl = "hello urtc"; // 如果是文字水印为水印内容   如果是图片则为图片url 地址
recordconfig.mMixerTemplateType = 4; [混流模板](http:https://github.com/UCloudDocs/urtc/blob/master/cloudRecord/RecordLaylout.md)
m_rtcengine->startRecord(recordconfig);
```


## 1.6 版本发布

* 1、加入大班课功能，支持超大规模会议功能
使用如下
``` c++ 
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```


## 1.5 版本

### 功能更新
* 1、支持x64为应用程序
* 2、优化日志上报，减少日志上报性能消耗
* 3、支持自定义渲染模式
调用
``` c++ 
实现 自定义渲染类 现在数据格式为 argb 格式输出
    class VideoRender : public UCloudRtcExtendVideoRender {
    public:
        virtual  void onRemoteFrame(const tUCloudRtcVideoFrame* videoframe)
        {
        }
    };

    tUCloudRtcVideoCanvas canvas;
    canvas.mVideoView = (void*)render; // mVideoView 由int 变更为 void*
    
    canvas.mRenderMode = UCLOUD_RTC_RENDER_MODE_FIT;
    canvas.mUserId = "";
    canvas.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;
    canvas.mRenderType = UCLOUD_RTC_RENDER_TYPE_EXTEND; // 自定义渲染类型

    m_rtcengine->startPreview(canvas);
``` 
* 4、视频外部采集支持有yuv420 扩展到 yuv420 rgb argb rgba 等格式，使用如下
``` c++ 
实现 自定义渲染类 现在数据格式为 argb 格式输出
    class VideoRender : public UCloudRtcExtendVideoCaptureSource {
    public:
      bool CSdkTestDemoDlg::doCaptureFrame(tUCloudRtcVideoFrame* videoframe)
        {

            if (!bSuccess)
                return false;
            if (videoframe)
            {
                videoframe->mDataBuf = dataBuffer; // 外部数据内存块
                videoframe->mHeight = 360; // 自定义数据 高度
                videoframe->mWidth = 640;// 自定义数据 宽度
                videoframe->mVideoType = UCLOUD_RTC_VIDEO_FRAME_TYPE_ARGB; // 自定外部输入数据类型
            }
            return true;
        }
    };

    m_rtcengine->enableExtendVideocapture(true, this); // 启用外部输入

    发布视频流  数据会按照固定帧率进行 采集

```


## 1.4 版本

### 功能更新
* 1、优化抗丢包能力，减少卡顿率
* 2、完善日志上报，提供更完善的监控指标查看
* 3、桌面采集和摄像头采集 支持rtsp 视频输入替换
调用如下：
``` c++  
type 表示替换摄像头还是摄像头  enable true 代表启用功能 rtspurl：要输入的url 地址  支持（vp8 h264编码格式）
virtual int enableExtendRtspVideocapture(eUCloudRtcMeidaType type, bool enable, const char* rtspurl) = 0;
``` 
* 4、增加入会前mute 摄像头和麦克风入会，适应更多场景需求
接口如下：
``` c++  
virtual int muteCamBeforeJoin(bool mute) = 0; // mute 摄像头
virtual int muteMicBeforeJoin(bool mute) = 0; // mute 麦克风
``` 
* 5、增加静音接口 以及会中切换摄像头功能
``` c++  
virtual int enableAllAudioPlay(bool enable) = 0; // 关闭应用声音
virtual int switchCamera(tUCloudRtcDeviceInfo& info) = 0; // 切换摄像头
``` 
* 6、增加编码格式设置支持 vp8 h264（264支持硬编解码  1080p25帧编码只需 15%CPU， 满足同时两路1080p 上传）
``` c++  
virtual int setVideoCodec(eUCloudRtcCodec codec) = 0; // 初始化后调用
``` 
* 7、增加渲染方式渲染  支持 gdi directx 渲染方式
通过view 里面的 mRenderType设置
``` c++  
virtual int startPreview(tUCloudRtcVideoCanvas& view) = 0;
virtual int startRemoteView(tUCloudRtcVideoCanvas& view) = 0;
``` 
* 8、录制接口 支持设置自己的 bucket region  
``` c++ 
调用如下： 
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型
recordconfig.mMainviewuid = m_userid.data(); // 主画面
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD; // 录制等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;
recordconfig.mBucket = "your bucket";
recordconfig.mBucketRegion = "your region";
m_rtcengine->startRecord(recordconfig);
``` 

### 接口变更

* 1、开始录制回调接口  回调增加录制文件名 bucket region 参数方便用户保持录制信息到自己的业务服务器
``` c++ 
virtual void onStartRecord(const int code, const char* msg, const char* recordid) {}
变更为
virtual void onStartRecord(const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 
* 2、设备测试模块 
``` c++ 
增加接口
virtual int setVideoDevice(tUCloudRtcDeviceInfo* info) = 0;
virtual int setRecordDevice(tUCloudRtcDeviceInfo* info) = 0;
virtual int setPlayoutDevice(tUCloudRtcDeviceInfo* info) = 0;
音频测试接口去掉了 id 设置  前提通过上面接口进行设备设置 然后调用测试接口
virtual int startRecordingDeviceTest(UCloudRtcAudioLevelListener* audiolevel) = 0;
virtual int startPlaybackDeviceTest(const char* testAudioFilePath) = 0;
``` 
``` c++ 
设备初始化大体流程如下
m_mediacallback = new MediaCallback(this->GetSafeHwnd());
m_mediadevice = UCloudRtcMediaDevice::sharedInstance();

m_mediadevice->InitAudioMoudle();
m_mediadevice->InitVideoMoudle();

int num = m_mediadevice->getCamNums();
for (int i=0; i<num; i++)
{
    tUCloudRtcDeviceInfo info;
    memset(&info, 0, sizeof(tUCloudRtcDeviceInfo));
    int ret = m_mediadevice->getVideoDevInfo(i, &info);
    if (ret == 0)
    {
        m_videolist.push_back(info);
        m_videocom.AddString(Utf8ToWide(info.mDeviceName).data());
    }
}
int audionum = m_mediadevice->getRecordDevNums();
for (int i = 0; i < audionum; i++)
{
    tUCloudRtcDeviceInfo info;
    memset(&info, 0, sizeof(tUCloudRtcDeviceInfo));
    int ret = m_mediadevice->getRecordDevInfo(i, &info);
    if (ret == 0)
    {
        if (i == 0)
        {
            m_mediadevice->setRecordDevice(&info);
        }
    }
}

int speakernum = m_mediadevice->getPlayoutDevNums();
for (int i = 0; i < audionum; i++)
{
    tUCloudRtcDeviceInfo info;
    memset(&info, 0, sizeof(tUCloudRtcDeviceInfo));
    int ret = m_mediadevice->getPlayoutDevInfo(i, &info);
    if (ret == 0)
    {
        if (i == 0)
        {
            m_mediadevice->setPlayoutDevice(&info);
        }
    }
}

int micvol = 0;
int ret = m_mediadevice->getRecordingDeviceVolume(&micvol);
m_micvol.SetPos(micvol);

int playvol = 0;
ret = m_mediadevice->getPlaybackDeviceVolume(&playvol);
m_speakervol.SetPos(playvol);
``` 

* 3、桌面采集增加窗口采集功能，调用用下
``` c++ 
m_rtcengine->setUseDesktopCapture(UCLOUD_RTC_DESKTOPTYPE_SCREEN); // 必须先调用 决定采集窗口还是桌面
int num = m_rtcengine->getWindowNums();// 获取窗口数量
for(int i=0; i<num; i++)
{
    //m_rtcengine->getWindowInfo(i, info); 
}
m_rtcengine->getWindowInfo(0, info); 
tUCloudRtcScreenPargram pgram;
pgram.mScreenindex = info.mDesktopId; //窗口标识  
pgram.mXpos = 0;
pgram.mYpos = 0;
pgram.mWidth = 0;
pgram.mHeight = 0;
m_rtcengine->setCaptureScreenPagrams(pgram);
``` 

* 4、桌面采集增加获取信息功能
``` c++ 
m_rtcengine->setUseDesktopCapture(UCLOUD_RTC_DESKTOPTYPE_SCREEN); // 必须先调用 决定采集窗口还是桌面
int num = m_rtcengine->getDesktopNums();// 获取桌面数量
for(int i=0; i<num; i++)
{
   // m_rtcengine->getDesktopInfo(i, info);
}
m_rtcengine->getDesktopInfo(0, info); 
tUCloudRtcScreenPargram pgram;
pgram.mScreenindex = info.mDesktopId; //桌面标识  
pgram.mXpos = 0;
pgram.mYpos = 0;
pgram.mWidth = 0;
pgram.mHeight = 0;
m_rtcengine->setCaptureScreenPagrams(pgram);
``` 
* 5、声音采集添加3通道支持 通道支持个数包括 1 2 3 4 播放支持 1 2 通道

## 1.3 版本

* 1、优化抗丢包能力，视频最多抗30%丢包，音频最高抗70%丢包
* 2、增加音频文件输入功能，可以替换micphone 输入，支持mp3 wav文件格式 接口 
``` c++
startAudioMixing(const char* filepath, bool replace, bool loop,float musicvol)
``` 
* 3、增加音频数据获取功能，支持用户直接获取播放采集音频数据
``` c++
void regAudioFrameCallback(UCloudRtcAudioFrameCallback* callback) 
``` 
* 4、优化设备模块，音频和视频模块可以分别单独启用 变更后启动引擎调用方式
``` c++
m_mediadevice = UCloudRtcMediaDevice::sharedInstance();
m_mediadevice->InitAudioMoudle();
m_mediadevice->InitVideoMoudle();
``` 
* 5、减少库体积 减少依赖库 解决curl ssl 库和用户自己curl ssl 冲突问题 
* 6、引擎获取接口改变  
``` c++
UCloudRtcEngine *sharedInstance(UCloudRtcEventListener* listener)更改为 UCloudRtcEngine *sharedInstance() 事件监听通过regRtcEventListener(UCloudRtcEventListener* listener)进行注册 
``` 


## 1.2 版本

* 1、优化抗丢包能力，30%丢包通话基本连续
* 2、增加录制功能，最多支持6路混流录制
* 3、设备测试模块增加视频数据获取功能，方便加入美颜等视频前置处理功能
* 4、增加媒体自动重连能力，并增加视频数据数据实时回调接口
``` c++
int startCaptureFrame(eUCloudRtcVideoProfile profile,UCloudRtcVideoFrameObserver* observer)
``` 
用户可以通过UCloudRtcVideoFrameObserver 获取视频数据 处理后通过UCloudRtcExtendVideoCaptureSource
将数据再次输入到引擎，实现美颜处理
* 5、增加音频数据能量回调功能
* 6、增加外部媒体采集扩展能力
``` c++
int enableExtendVideocapture(bool enable, UCloudRtcExtendVideoCaptureSource* videocapture)
``` 
用户通过实现 UCloudRtcExtendVideoCaptureSource 接口实现外部输入源导入 实现视频前置处理

## 1.1 版本

* 1、增加桌面分享功能，并支持指定区域分享
* 2、增加sdk 模式设置，区分测试 正式模式方便测试
* 3、优化内存使用，内存占比减少 30% 
* 4、优化重连效果



## 1.0 版本
* 1、支持1对1 1对多音视频通话  单房间支持最多15人 6人同时连麦
* 2、支持纯音频通话模式
* 3、抗丢包 15%  
* 4、断线制动重连
* 5、支持配置自动发布模式
