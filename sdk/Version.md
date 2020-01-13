# 版本说明

<!-- tabs:start -->

# ** web **

## 1.4.6版

该版本发布于 2020-01-03。

1. 开放私有化部署时针对房间服务器部署与否两种场景的设置
2. 添加 types 文件，支持用 typescript 调用 sdk 编写代码
3. 新增获取本地浏览器支持的音视频编码格式的功能
4. 修复 iOS safari, 360 等浏览器无法发布的问题，已经支持的浏览器：
 - 支持Chrome 60及以上版本
 - 支持Safari 11及以上版本
 - 支持Firefox 56及以上版本
 - 支持opera 50及以上版本
 - 支持QQ浏览器 10及以上版本
 - 支持360安全浏览器 10及以上版本
 - 支持360极速浏览器 12及以上版本
5. 修复日志上报参数错误的问题
6. 修复远端 mute 视频时，本地展示画面不黑屏，停留在最后一帧的问题
7. 完善/修正 API 文档

## 1.4.5版

该版本发布于 2019-12-17。

1. 修复回音问题

## 1.4.4版

该版本发布于 2019-12-16。

sdk未做变更，此次发布仅用于解决 1.4.3 未发布成功的问题

## 1.4.3版

该版本发布于 2019-12-16。

1. 修复切换音频时报错的问题
2. 新增设备可用性检测方法

## 1.4.2版

该版本发布于 2019-12-16。

1. 修复订阅流有时无法被正常订阅的问题
2. 修复错误日志打印异常的问题
3. 修正 API 文档中关于 publish 的功能描述

## 1.4.1版

该版本发布于 2019-12-13。

1. 新增预览的 API
2. 新增设置音频音量的 API
3. 修复播放音效时"回声"的问题

## 1.4.0版

该版本发布于 2019-12-11。

1. 支持同时推两路流（一路摄像头，一路屏幕共享）
2. 新增 getLocalStreams 方法, 新增 getRemoteStreams 方法（用于替代 getStreams 方法）
3. 新增 getMediaStream 方法，用于替代 getLocalMediaStream 和 getRemoteMediaStream 方法
4. 修复丢包率获取有误的问题
5. 修复取消订阅后，再次订阅不成功的问题
6. 修复多次监听到同一条流的 stream-published / stream-subscribed 事件通知的问题
7. 优化 snapshot 方法，使其更易使用，并支持由用户指定图片文件名进行下载（注：此方法的调用方式无法向前兼容）

## 1.3.10版

该版本发布于 2019-12-06。

1. 允许用户自定义混流录制时输出的视频的带高
2. snapshot 文档完善

## 1.3.9版

该版本发布于 2019-12-05。

1. 修复设置视频 profile 不生效的问题
2. 修复屏幕共享无法设置视频宽高的问题

## 1.3.7版

该版本发布于 2019-12-02。

1. 优化屏幕共享功能 - 新增屏幕共享热切换（不需要取消发布后重新发布）
2. 新增图片热切换 - 可将静态图片文件转为视频流进行推送
4. 新增视频截屏功能（可直接保存图片到本地）
5. 修复订阅流无法混音的问题
6. 优化混音功能，支持热切换（切换麦克风、摄像头、屏幕共享、图片）时，混音不中断
7. 修复初次获取音视频设备（未进行浏览器授权）时，无法获取设备 label 描述信息的问题

## 1.3.6版

该版本发布于 2019-11-28。

1. 修复无法订阅纯音频的bug
2. 解决获取设备信息时，不能获取设备描述信息的问题
3. 完善水印的文档说明
4. 添加混音功能
5. 支持 mute/unmute 订阅媒体流
6. 优化屏幕共享功能

## 1.3.5版

该版本发布于 2019-11-27。

1. 增加水印功能
2. 设备不可用时可能导致发布失败的bug修复
3. 找不到 UCloudRTC 暴露的 generateToken 的方法的问题修复

## 1.3.4版

该版本发布于 2019-11-25。

1. 修复网络状态信息采集的bug
2. 屏幕共享 - 支持低版本（72以下）Chrome 浏览器以插件的方式进行
3. 完善/修正API文档

## 1.3.3版

该版本发布于 2019-11-20。

1. 日志上报服务变更
2. 修复订阅流的网络状态数据获取有误的问题

## 1.3.2版

该版本发布于 2019-11-18。

1. 日志上报服务变更
2. API 文档修正

## 1.3.1版

该版本发布于 2019-11-15。

1. 修复无法 muteVideo 的问题

## 1.3.0版

该版本发布于 2019-11-14。

1. joinRoom 方法的成功回调改为返回当前房间其他用户及正在发布的流的信息
2. leaveRoom 方法的成功回调不再返回冗余数据
3. publish 去除无效的成功回调函数，发布的成功以 stream-published 事件为准
4. subscribe 去除无效的成功回调函数，订阅的成功以 stream-subscribe 事件为准
5. setServers 配置 log 的地址设为空字符串时，可关闭日志上报功能
6. 规范日志上报的设备信息格式
7. API 文档中，添加水印功能暂未开放的提示
8. 修复 Safari 浏览器推流时，Chrome 浏览器订阅成功但不能显示视频的Bug
9. 修复 Safari 浏览器连续多次发布及取消发布后代码报错的问题

## 1.2.3版

该版本发布于 2019-11-11。

1. 修正日志上报的 DeviceInfo

## 1.2.2版

该版本发布于 2019-11-11。

1. 默认房间类型改为 rtc
2. 默认认角色类型为 push-and-pull
3. 修改 offer 中的替换部分

## 1.2.1版

该版本发布于 2019-11-08。

1. 解决摄像头和屏幕共享切换的问题
2. 完善 API 文档
3. 添加 sdk 使用说明文档

## 1.2.0版

该版本发布于 2019-11-08。

1. 更改 genToken 方法名为 generateToken
2. 修正日志上报时的 lostpre 问题
3. 新增屏幕共享功能
4. 解决 chrome 和 firefox 无法订阅 safari 发布的流的问题

## 1.1.2版

该版本发布于 2019-11-07。

1. 对外暴露临时生成 token 的方法

## 1.1.1版

该版本发布于 2019-11-07。

1. 初次发布

# ** Windows **

## 1.6.1版

该版本发布于2019-12-3。  

1、加入大班课功能，支持超大规模会议功能    

使用如下：

``` c++ 
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```
2、添加自定义编码（最大分辨率到1080p(1920*1080)）    

``` c++ 
virtual void setVideoProfile(eUCloudRtcVideoProfile profile) = 0;
变更为
virtual void setVideoProfile(eUCloudRtcVideoProfile profile, tUCloudVideoConfig& videoconfig) = 0;
tUCloudVideoConfig 用来定义自己编码分辨率
```
3、录制功能增加更多录制模板以及水印 调用示例如下：    

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


## 1.6版

该版本发布于2019-11-14。  

1、加入大班课功能，支持超大规模会议功能    

使用如下：

``` c++ 
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```


## 1.5版

该版本发布于2019-11-7。  

### 功能更新
1、支持x64为应用程序  
2、优化日志上报，减少日志上报性能消耗  
3、支持自定义渲染模式  
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
4、视频外部采集支持有yuv420 扩展到 yuv420 rgb argb rgba 等格式，使用如下：    

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

该版本发布于2019-10-30。  

### 功能更新
1、优化抗丢包能力，减少卡顿率    
2、完善日志上报，提供更完善的监控指标查看    
3、桌面采集和摄像头采集 支持rtsp 视频输入替换    
调用如下：   

``` c++  
type 表示替换摄像头还是摄像头  enable true 代表启用功能 rtspurl：要输入的url 地址  支持（vp8 h264编码格式）
virtual int enableExtendRtspVideocapture(eUCloudRtcMeidaType type, bool enable, const char* rtspurl) = 0;
``` 
4、增加入会前mute 摄像头和麦克风入会，适应更多场景需求    
接口如下：    

``` c++  
virtual int muteCamBeforeJoin(bool mute) = 0; // mute 摄像头
virtual int muteMicBeforeJoin(bool mute) = 0; // mute 麦克风
``` 
5、增加静音接口 以及会中切换摄像头功能    

``` c++  
virtual int enableAllAudioPlay(bool enable) = 0; // 关闭应用声音
virtual int switchCamera(tUCloudRtcDeviceInfo& info) = 0; // 切换摄像头
``` 
6、增加编码格式设置支持 vp8 h264（264支持硬编解码  1080p25帧编码只需 15%CPU， 满足同时两路1080p 上传）    

``` c++  
virtual int setVideoCodec(eUCloudRtcCodec codec) = 0; // 初始化后调用
``` 
7、增加渲染方式渲染  支持 gdi directx 渲染方式    

通过view 里面的 mRenderType设置    

``` c++  
virtual int startPreview(tUCloudRtcVideoCanvas& view) = 0;
virtual int startRemoteView(tUCloudRtcVideoCanvas& view) = 0;
``` 

8、录制接口 支持设置录制的 bucket region  

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

1、开始录制回调接口  回调增加录制文件名 bucket region 参数方便用户保持录制信息到自己的业务服务器。    

``` c++ 
virtual void onStartRecord(const int code, const char* msg, const char* recordid) {}
变更为
virtual void onStartRecord(const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 
2、设备测试模块    

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

3、桌面采集增加窗口采集功能，调用用下：    

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

4、桌面采集增加获取信息功能    

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
5、声音采集添加3通道支持 通道支持个数包括 1 2 3 4 播放支持 1 2 通道。    

## 1.3版

该版本发布于2019-8-22。  

1、优化抗丢包能力，视频最多抗30%丢包，音频最高抗70%丢包    
2、增加音频文件输入功能，可以替换micphone 输入，支持mp3 wav文件格式 接口    

``` c++
startAudioMixing(const char* filepath, bool replace, bool loop,float musicvol)
``` 

3、增加音频数据获取功能，支持用户直接获取播放采集音频数据    

``` c++
void regAudioFrameCallback(UCloudRtcAudioFrameCallback* callback) 
``` 
4、优化设备模块，音频和视频模块可以分别单独启用 变更后启动引擎调用方式    

``` c++
m_mediadevice = UCloudRtcMediaDevice::sharedInstance();
m_mediadevice->InitAudioMoudle();
m_mediadevice->InitVideoMoudle();
``` 
5、减少库体积 减少依赖库 解决curl ssl 库和用户自己curl ssl 冲突问题   
6、引擎获取接口改变    

``` c++
UCloudRtcEngine *sharedInstance(UCloudRtcEventListener* listener)更改为 UCloudRtcEngine *sharedInstance() 事件监听通过regRtcEventListener(UCloudRtcEventListener* listener)进行注册 
``` 


## 1.2版

该版本发布于2019-7-8。  

1、优化抗丢包能力，30%丢包通话基本连续   
2、增加录制功能，最多支持6路混流录制   
3、设备测试模块增加视频数据获取功能，方便加入美颜等视频前置处理功能   
4、增加媒体自动重连能力，并增加视频数据数据实时回调接口   

``` c++
int startCaptureFrame(eUCloudRtcVideoProfile profile,UCloudRtcVideoFrameObserver* observer)
``` 
用户可以通过UCloudRtcVideoFrameObserver 获取视频数据；处理后通过UCloudRtcExtendVideoCaptureSource，将数据再次输入到引擎，实现美颜处理。

5、增加音频数据能量回调功能    

6、增加外部媒体采集扩展能力   

``` c++
int enableExtendVideocapture(bool enable, UCloudRtcExtendVideoCaptureSource* videocapture)
``` 
用户通过实现 UCloudRtcExtendVideoCaptureSource 接口实现外部输入源导入，实现视频前置处理。

## 1.1版

该版本发布于2019-7-2。  

1、增加桌面分享功能，并支持指定区域分享    
2、增加sdk 模式设置，区分测试 正式模式方便测试    
3、优化内存使用，内存占比减少 30%    
4、优化重连效果   



## 1.0版

该版本发布于2019-6-26。  

1、支持1对1 1对多音视频通话  单房间支持最多15人 6人同时连麦    
2、支持纯音频通话模式   
3、抗丢包 15%     
4、断线制动重连   
5、支持配置自动发布模式   





<!-- tabs:end -->
