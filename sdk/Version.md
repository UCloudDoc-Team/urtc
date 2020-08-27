# 版本说明

<!-- tabs:start -->

# ** Web **


## 1.6.3版

该版本发布于 2020-08-26。

1. 优化对 typescript 的支持

## 1.6.2版

该版本发布于 2020-08-20。

1. 开启录制/转推 API 新增参数 keyUser，用于指定关键用户停止推流时，停止录制/转推任务
2. 修订 API 文档

## 1.6.1版

该版本发布于 2020-08-17。

1. 修复指定摄像头设备 ID 以及 facingMode 时，导致可能无法发布的问题
2. 优化弱网检测自动切换信令功能

## 1.6.0版

该版本发布于 2020-08-11。

1. 修复 macOS 下 safari 浏览器发布采集麦克风的屏幕共享流时失败的问题
2. 新增用户自定义视频 profile 功能
3. 完善弱网时自动切换信令的功能

## 1.5.22版

该版本发布于 2020-07-31。

1. 新增 updateRelayPushURL 及 updateRelayLayout 方法

## 1.5.21版

该版本发布于 2020-07-30。

1. 优化 switchImage 方法，对部分资源跨域访问的支持
2. 日志上报内存数据对部分浏览器的支持

## 1.5.20版

该版本发布于 2020-07-24。

1. 修复有时无 user-added 事件的问题
2. 修复部分浏览器不支持采集屏幕共享时音频时报错的问题
3. 新增 enableAudioVolumeIndicator 方法及 volume-indicator 事件，用于定时报告远端流的音量
4. 添加 640*480_1, 1280*720_3, 1920*1080_3 三种5帧的 profile
5. 提高推图片流时的帧
6. 优化 trackst 与 streamst 时序错乱的问题

## 1.5.19版

该版本发布于 2020-07-13。

1. 修复开启录制/转推任务失败的问题
2. 修复极少部分外接摄像头无法正常访问的问题
3. 修复上报日志类型错误的问题
4. 优化屏幕共享功能，解决帧率较低，导致画面模糊的问题

## 1.5.18版

该版本发布于 2020-07-09。

1. 修复推图片流时，最小化浏览器后，后进入房间的用户的做法正常播放该流的问题
2. 修复发布后建连失败，但无法重连的问题


## 1.5.17版

该版本发布于 2020-07-03。

1. 修复 updateRecordStreams 和 updateRelayStreams 方法在 replace 模式下，未变更 mainViewUId 和 mainViewType 的问题
2. 修复 updateRecordStreams 和 updateRelayStreams 方法变更 streamAddMode 添流模式的问题
3. 修复录制/转推日志上报问题
4. 新增 "1280\*720_1", "1280\*720_2", "1920\*1080_1", "1920\*1080_2" 四种 profile


## 1.5.14版

该版本发布于 2020-06-24。

1. 修复 queryMix 查询未开启录制/转推任务的房间时，返回错误的问题
2. 修复 live 模式下屏幕共享时帧率升不上去的问题
3. 修复无 network-quality 报告的问题
4. 上报加入房间失败时的日志

## 1.5.10版

该版本发布于 2020-06-16。

1. 修复连续快速播放并停止播放播放时出现不能完全停止播放的问题
2. 新增 publishStream 和 unpublishStream 方法，用于发布/取消发布 createStream 创建的流
3. 将 removeStream 方法更名为 destroyStream
4. 修复 createStream 创建屏幕共享流时可能会关闭麦克风失败的问题

## 1.5.9版

该版本发布于 2020-06-16。

1. 新增 updateRecordStreams 及 updateRelayStreams 的 replace 操作类型，用于完整替换（切换）录制/转推的流

## 1.5.8版

该版本发布于 2020-06-12。

1. 新增 setVideoProfile 方法，可用于未发布的流的video profile的更改。


## 1.5.7版

该版本发布于 2020-06-12。

1. 新增 removeStream 方法，用于删除通过 createStream 创建且未发布的流
2. 修复 createStream 创建的屏幕共享预览流，无 screenshare-stopped 事件通知的问题
3. 修复 resume 后 getAudioVolume 仍无法获取正确音量的问题
4. 修复 mute 订阅流未通知到服务器的问题
5. 修复未订阅流时，收到 mute 信令，但 sdk 未修正该流的 mute 状态的问题
6. 修复通过 createStream 创建的流未暴露 mediaStream 的问题
7. 修复部分浏览器在屏幕共享时报错而无法推流的问题

## 1.5.6版

该版本发布于 2020-06-08。

1. 新增 stop 方法，用于停止播放一条流
2. 新增 createStream 方法，用于创建本地（预览）流，便于预览及直接发布
3. 修复对后台返回 null 消息时的处理
4. 修复对后台发送多余 userst 消息的处理

## 1.5.5版

该版本发布于 2020-05-28。

1. 优化断网重连后，对重推的流的 audio 或 video 恢复为重连前的状态
2. 优化对于订阅流无法进行订阅时的处理

## 1.5.4版

该版本发布于 2020-05-25。

1. 优化断线重连，修复断线前发出的信令不执行回调的问题
2. 修复 chrome 浏览器流重连时音视频质量无法立即恢复的问题
3. 修复使用 play 方法进行播放后，用户主动消除容器导致 player destroy 时报错的问题
4. 修复 record-notify 和 relay-notify 对于成功消息进行错误处理的问题


## 1.5.3版

该版本发布于 2020-05-15。

1. 允许业务侧设置播放元素的控制面板的显示或隐藏
2. 修复 play 方法，传入错误参数不报错的问题
3. 修正订阅某条流失败时的处理逻辑



## 1.5.2版

该版本发布于 2020-05-07。

1. 修复调用 preview 方法进行预览时，显示黑屏的问题
2. 修复调用 deviceDetection 方法检测时，返回值不正确的问题
3. 规范 leaveroom 的错误码



## 1.5.1版

该版本发布于 2020-05-06。

1. 修复录制/转推中关于码率调整的Bug
2. 修正网络质量评估功能，提高评分的准确性
3. 优化兼容性，部分老版本的Edge浏览器支持发布及订阅音频功能
4. 优化日志上报，修复其导致部分 Firefox 浏览器渲染视频时的卡顿现象
5. 优化断线自动重连功能，提高断线检测的灵敏度



## 1.5.0版

该版本发布于 2020-04-20。

1. 支持发布图片流
2. 新增startRecord, stopRecord, updateRecordStreams, startRelay, stopRelay, updateRelayStreams 方法，用于录制、旁路推流功能
3. 录制/转推新增 customMain, customFlow, single 的布局
4. 新增 record-notify 及 relay-notify 事件，用于通知录制/转推中的错误
5. 新增 setRole 方法，允许 live 模式下，切换角色，进行连麦
6. 对本地浏览器不支持的编码格式的流进行订阅时，将会收到订阅失败的错误提示
7. 新增 play, resume 方法，播放音视频流时，无需用户自己操作 video 和 audio 标签，使播放更简单
8. 发布流增加音轨，允许用户针对该音轨进行处理，如展示声音的波形图等
9. 其他一些问题的修复及内部优化

## 1.4.20版

该版本发布于 2020-03-27。

1. 支持 iOS 下的浏览器播放。
2. 添加对自动重连的处理


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

## 1.6.30版

该版本发布于2020-6-19。  

1、增加桌面发布profile 可对应不同的分辨率及发送码率。


## 1.6.11版

该版本发布于2020-4-27。  

1、新增远端音频播放首帧回调  
2、新增远端视频首帧回调  
3、桌面流可加声音  
4、新增registerAudioFrameObserver  用于采集数据回调替换  
5、新增registerVideoFrameObserver 用于采集数据回调替换  
6、新增muteRomoteCamBeforeSub 用于订阅前mute远端cam  
7、新增muteRomoteMicBeforeSub 用于订阅前mute远端mic  


## 1.6.5版

该版本发布于2020-3-26。  

1、加入cdn旁路推流功能
2、录制功能更新可以指定混流得流和模板，可以兼容之前版本  

## 1.6.1版

该版本发布于2019-12-3。  

1、加入大班课功能，支持超大规模会议功能    

使用如下：

```cpp
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```
2、添加自定义编码（最大分辨率到1080p(1920*1080)）    

```cpp
virtual void setVideoProfile(eUCloudRtcVideoProfile profile) = 0;
变更为
virtual void setVideoProfile(eUCloudRtcVideoProfile profile, tUCloudVideoConfig& videoconfig) = 0;
tUCloudVideoConfig 用来定义自己编码分辨率
```
3、录制功能增加更多录制模板以及水印 调用示例如下：    

```cpp
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

```cpp
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

```cpp
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

```cpp
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

```cpp
type 表示替换摄像头还是摄像头  enable true 代表启用功能 rtspurl：要输入的url 地址  支持（vp8 h264编码格式）
virtual int enableExtendRtspVideocapture(eUCloudRtcMeidaType type, bool enable, const char* rtspurl) = 0;
``` 
4、增加入会前mute 摄像头和麦克风入会，适应更多场景需求    
接口如下：    

```cpp 
virtual int muteCamBeforeJoin(bool mute) = 0; // mute 摄像头
virtual int muteMicBeforeJoin(bool mute) = 0; // mute 麦克风
``` 
5、增加静音接口 以及会中切换摄像头功能    

```cpp

virtual int enableAllAudioPlay(bool enable) = 0; // 关闭应用声音
virtual int switchCamera(tUCloudRtcDeviceInfo& info) = 0; // 切换摄像头
``` 
6、增加编码格式设置支持 vp8 h264（264支持硬编解码  1080p25帧编码只需 15%CPU， 满足同时两路1080p 上传）    

```cpp 
virtual int setVideoCodec(eUCloudRtcCodec codec) = 0; // 初始化后调用
``` 
7、增加渲染方式渲染  支持 gdi directx 渲染方式    

通过view 里面的 mRenderType设置    

```cpp
virtual int startPreview(tUCloudRtcVideoCanvas& view) = 0;
virtual int startRemoteView(tUCloudRtcVideoCanvas& view) = 0;
``` 

8、录制接口 支持设置录制的 bucket region  

```cpp
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

```cpp
virtual void onStartRecord(const int code, const char* msg, const char* recordid) {}
变更为
virtual void onStartRecord(const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 
2、设备测试模块    

```cpp 
增加接口
virtual int setVideoDevice(tUCloudRtcDeviceInfo* info) = 0;
virtual int setRecordDevice(tUCloudRtcDeviceInfo* info) = 0;
virtual int setPlayoutDevice(tUCloudRtcDeviceInfo* info) = 0;
音频测试接口去掉了 id 设置  前提通过上面接口进行设备设置 然后调用测试接口
virtual int startRecordingDeviceTest(UCloudRtcAudioLevelListener* audiolevel) = 0;
virtual int startPlaybackDeviceTest(const char* testAudioFilePath) = 0;
``` 
```cpp 
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

```cpp
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

```cpp
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

```cpp
startAudioMixing(const char* filepath, bool replace, bool loop,float musicvol)
``` 

3、增加音频数据获取功能，支持用户直接获取播放采集音频数据    

```cpp
void regAudioFrameCallback(UCloudRtcAudioFrameCallback* callback) 
``` 
4、优化设备模块，音频和视频模块可以分别单独启用 变更后启动引擎调用方式    

```cpp
m_mediadevice = UCloudRtcMediaDevice::sharedInstance();
m_mediadevice->InitAudioMoudle();
m_mediadevice->InitVideoMoudle();
``` 
5、减少库体积 减少依赖库 解决curl ssl 库和用户自己curl ssl 冲突问题   
6、引擎获取接口改变    

```cpp
UCloudRtcEngine *sharedInstance(UCloudRtcEventListener* listener)更改为 UCloudRtcEngine *sharedInstance() 事件监听通过regRtcEventListener(UCloudRtcEventListener* listener)进行注册 
``` 


## 1.2版

该版本发布于2019-7-8。  

1、优化抗丢包能力，30%丢包通话基本连续   
2、增加录制功能，最多支持6路混流录制   
3、设备测试模块增加视频数据获取功能，方便加入美颜等视频前置处理功能   
4、增加媒体自动重连能力，并增加视频数据数据实时回调接口   

```cpp
int startCaptureFrame(eUCloudRtcVideoProfile profile,UCloudRtcVideoFrameObserver* observer)
``` 
用户可以通过UCloudRtcVideoFrameObserver 获取视频数据；处理后通过UCloudRtcExtendVideoCaptureSource，将数据再次输入到引擎，实现美颜处理。

5、增加音频数据能量回调功能    

6、增加外部媒体采集扩展能力   

```cpp
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

1. 支持1对1 1对多音视频通话  单房间支持最多15人 6人同时连麦    
2. 支持纯音频通话模式   
3. 抗丢包 15%     
4. 断线制动重连   
5. 支持配置自动发布模式 



# ** Android **

## 1.7.6

该版本发布于2020-5-14，sdk 1.7.6。  
更新内容：  
1.优化蓝牙的问题

## 1.7.5

该版本发布于2020-4-28，sdk 1.7.5。  
更新内容：    
1.增加调整录音音量 arm7 arm8   
2.增加网络质量回调

```java
/**
     * 网络质量监控回调
     * @param userId 用户id
     * @param streamType 流类型 {@link UCloudRtcSdkStreamType}
     * @param mediaType 媒体类型 {@link UCloudRtcSdkMediaType}
     * @param quality 质量监控 {@link UCloudRtcSdkNetWorkQuality}
     */
    void onNetWorkQuality(String userId,UCloudRtcSdkStreamType streamType,UCloudRtcSdkMediaType mediaType,UCloudRtcSdkNetWorkQuality quality);
```

3.增加maven 仓库    
4.增加修改混流接口

```java
/**
     * 开始服务端混流转推
     * @param mixProfile 混流参数配置，参见{@link UCloudRtcSdkMixProfile}
     */
    void startMix(UCloudRtcSdkMixProfile mixProfile);

    /**
     * 结束混流转推
     * @param type mixprofile中设置的推流类型
     * @param pushUrls mixprofile中pushurl的子集，需要结束推流的地址集合
     */
    void stopMix(int type, JSONArray pushUrls);

    /**
     * 动态添加混流
     * @param streams 需要被添加推流的流信息集合
     */
    void addMixStream(JSONArray streams);

    /**
     * 动态删除混流
     * @param streams 需要被取消推流的流信息集合
     */
    void delMixStream(JSONArray streams);
```
5.修改镜像功能，支持前置摄像头镜像。    


## 1.7.3版

该版本发布于2020-4-5，sdk 20120511 1.7.3    

更新内容：    
* 增加android 4.3 的兼容
* 增加本地渲染view，远端view首帧渲染接口回调
* 增加本地渲染view，远端view截屏回调
* 增加URTCRenderView的单纯surfaceview控件
* 增加TextureView渲染控件支持
* 增加本地录音重采样回调
* 增加后台和锁屏的操作接口，可以用来暂停和恢复音视频模块
* 完善断线重连机制
* 增加cdn转推部分接口

## 1.7.1版

该版本发布于 2020-03-20，sdk ucloudrtclib_1.7.1_0bd0905e_w    
更新内容：增加旁路推流的功能，及其相关接口，更多参数请参见api文档。    

    
## 1.7.0版

该版本发布于 2020-03-18，sdk ucloudrtclib__1.7.0_8e21045c_w    

更新内容：    
1.完善断线重连机制，增加远端断线回调，app可以根据此回调显示遮盖物等操作，注意此回调是确定远端断线后的回调，当远端出现网络失去连接等情况时还有机会连上，等到一定时间后，大约10几秒sdk确定远端无法连接上后会给与回调。    

```java
/**
     * peer 失去连接回调，给app用以做场景化的处理
     * @param type 本地还是远端，对应{@link UCloudRtcSdkStreamType}
     * @param info 连接相关信息
     */
    void onPeerLostConnection(int type ,UCloudRtcSdkStreamInfo info);
```
2.增加后台和锁屏的操作接口，后台和锁屏分两种情况。    
第一种情况，如果需要保持后台和锁屏时通信不断，需要使用前台service机制，在app退到后台时候开启前台service可以保证摄像头不中断。    
第二种情况如果需要保持通信不断，请在退到后台时候调用controlLocalVideo（false）controlAudio（false），后台期间可以正常执行其它录音，打电话的操作，回到前台时调用controlLocalVideo（true），controlAudio（true）恢复rtc通信。    

```java
/**
     * @param enable true启用 false禁用
     * 用于后台或者锁屏时候暂停恢复音频模块
     */
    void controlAudio(boolean enable);
/**
     *  @param enable true 启用 false禁用
     * 用于后台或者锁屏时候暂停恢复本地视频模块
     */
    void controlLocalVideo(boolean enable); 
```

3.增加推流时横竖屏固定和自动模式选择，譬如在横屏模式下退到后台，同时调用controlLocalVideo（false），可能因为手机界面从横屏activity切换到竖屏lunch时屏幕方向旋转，导致推流的画面最后一帧出现旋转。UCLOUD_RTC_PUSH_LANDSCAPE_MODE就可以避免出现这样的情况。    

```java
 /**
     * 自动模式，会根据设备方向和摄像头方向进行计算
     */
    UCLOUD_RTC_PUSH_AUTO_MODE,

    /**
     * 固定横屏模式，固定设备方向为90°，无视手机在竖屏方向上的旋转
     */
    UCLOUD_RTC_PUSH_LANDSCAPE_MODE,

    /**
     * 固定竖屏方式，固定了设备方向为0°，无视手机在横屏方向上的旋转
     */
    UCLOUD_RTC_PUSH_PORTRAIT_MODE 
```

4.修复textureview渲染某些情况下的异常。    


## 1.6.7版

该版本发布于 2020-02-18，sdk ucloudrtclib_1.6.7_zy_d5311475    

新增16k 音频采样功能。    

## 1.6.7版  

该版本发布于 2020-02-13   
新增渲染模式设置.  

```java
/**
 * SDK 渲染图像伸缩方式
 */
public enum UCloudRtcSdkScaleType {
    /**
     * 保持宽高比例，长边占满，短边按比例适应
     */
    UCLOUD_RTC_SDK_SCALE_ASPECT_FIT,
    /**
     * 保持宽高比例，平铺，短边占满，长边溢出裁剪
     */
    UCLOUD_RTC_SDK_SCALE_ASPECT_FILL,
    /**
     * 不保持宽高比例，画面直接拉伸或者缩放到view大小
     */
    UCLOUD_RTC_SDK_SCALE_FILL
}
```

 - 本地渲染     
 
```java
/**
​     * 开启本地预览
​     * @param mediatype 参见 {@link UCloudRtcSdkMediaType}
​     * @param renderView 渲染view，可以是sdk定义的{@Link UCloudRtcSdkSurfaceVideoView}或者textureview
​     * @param scaleType 渲染模式 参见{@link UCloudRtcSdkScaleType}
​     * @param callBack 可以为空，首帧渲染回调
​     * @return 错误码, 参见 {@link com.ucloudrtclib.sdkengine.define.UCloudRtcSdkErrorCode}
​     */
​    public UCloudRtcSdkErrorCode startPreview(UCloudRtcSdkMediaType mediatype, Object renderView,UCloudRtcSdkScaleType scaleType,UcloudRTCFirstFrameRendered callBack);
```

 - 远端渲染    
```java
/**
​     * 开启远端渲染   函数只在自己订阅成功获取远端流的信息后才能调用
​     * @param info 参见 {@link UCloudRtcSdkStreamInfo}
​     * @param renderview 参见 {@link UCloudRtcSdkSurfaceVideoView}
​     * @param scaleType 渲染模式 {@link UCloudRtcSdkScaleType}，若为null 即 ASPECT_FILL
​     * @param callBack 可以为空，首帧渲染回调
​     * @return 错误码，参见 {@link com.ucloudrtclib.sdkengine.define.UCloudRtcSdkErrorCode}
​     */
​    public UCloudRtcSdkErrorCode startRemoteView(UCloudRtcSdkStreamInfo info, Object renderview,UCloudRtcSdkScaleType scaleType, UcloudRTCFirstFrameRendered callBack);
```

 - 动态渲染，譬如之前某个view设置了ASPECT_FILL,可以在渲染的过程中动态修改为其它渲染方式。如果不在渲染过程中调用，则设置的模式直到下次渲染的时候生效

```java
/**
​     * 设置渲染模式
​     * @param isLocal 是否设置的本地推流渲染
​     * @param info 流信息
​     * @param scaleType 渲染模式
​     */
​    void setRenderViewMode(boolean isLocal,UCloudRtcSdkStreamInfo info, UCloudRtcSdkScaleType scaleType);
```


修复音画同步出现：    
现在sdk远端订阅声音播放会延迟到首帧渲染后在播放出来。    

修复离开房间，即调用leave channel不释放egl环境的bug。    

## 1.6.7版

该版本发布于 2020-02-10    

新增截图功能：    

```java
/**
 *给指定的流截屏
 * @param isLocal 是否本地截图
 * @param streamInfo 流信息
 * @param rtcScreenShotCallBack 截图回调
*/
 void takeSnapShot(boolean isLocal,UCloudRtcSdkStreamInfo streamInfo, UcloudRTCScreenShot rtcScreenShotCallBack);


/**
 * @author ciel
 * @create 2019/11/29
 * @Describe 截屏回调
 */

public interface UcloudRTCScreenShot {
​    /**
​     * 输出的截屏数据，rgba格式
​     * @param rgbBuffer 输出的数据
​     * @param width 输出数据的宽
​     * @param height 输出数据的高
​     */
​    void onReceiveRGBAData(ByteBuffer rgbBuffer, int width , int height);
}
```

确保sdk 调用此远程截图接口时已经成功订阅了流，即在onSubscribeResult返回订阅成功以后可以触发，否则调用无效。    

 - 远端截图
 
```java
mSdkEngine.takeSnapShot(false,viewInfo.getStreamInfo(), new UcloudRTCScreenShot() {
			@Override
                        public void onReceiveRGBAData(ByteBuffer rgbBuffer, int width, int height) {
                            Log.d(TAG, "onReceiveRGBAData: rgbBuffer: " + rgbBuffer + " width: " + width + " height: " + height);
                            final Bitmap bitmap = Bitmap.createBitmap(width * 1, height * 1, Bitmap.Config.ARGB_8888);

                            bitmap.copyPixelsFromBuffer(rgbBuffer);
                            String name = "/mnt/sdcard/urtcscreen_" + System.currentTimeMillis() + ".jpg";
                            File file = new File(name);
                            try {
                                FileOutputStream out = new FileOutputStream(file);
                                if (bitmap.compress(Bitmap.CompressFormat.JPEG, 100, out)) {
                                    out.flush();
                                    out.close();
                                }
                            } catch (FileNotFoundException e) {
                                e.printStackTrace();
                            } catch (IOException e) {
                                e.printStackTrace();
                            }
                            Log.d(TAG, "screen shoot : " + name);
                        }
```
 - 本地截图
 
这里的streaminfo 可以在 onLocalPublish的回调中获取，同样的本地截图需要确保在本地成功推流后，即在onLocalPublish成功后调用。    
takeScreenShot(true,streminfo).      

 - 首帧回调
 
```java
/**
 * @author ciel
 * @create 2020/2/10
 * @Describe 首帧渲染接口回调
 */
public interface UcloudRTCFirstFrameRendered {
    /**
     *
     * @param info 如果是本地渲染，info 是null,如果是远端渲染，info有数据
     * @param view 渲染的view
     *
     */
    void onFirstFrameRender(UCloudRtcSdkStreamInfo info, View view);
}
```

 - 本地首帧回调
 
```java
sdkEngine.startPreview(info.getMediaType(),
		localrenderview, (info, view) -> Log.d(TAG, "onLocalFirstFrameRender: " + "view: "+ view));
```

 - 远端首帧回调
 
```java
sdkEngine.startRemoteView(viewInfo.getStreamInfo(), 
	videoView,(info, view) -> Log.d(TAG, "onRemoteFirstFrameRender: " + "view: "+ view));
```

## 1.6.2版

该版本发布于2019-12-6，sdk 345a7545 1.6.2    

* 录像增加mainviewuid
* 增加重连次数和私有化部署相关接口


## 1.6.1版

该版本发布于2019-12-3，sdk bfafbf3c 1.6.1    

* 扩展录像功能，支持图片水印，文字水印，多种布局等，优化移动端录制视频旋转问题
* 限制扩展输入模式分辨率为720P，增加非指定分辨率输入的缩放
* 增加远端近端截图功能
* 修复已知bug


## 1.5.9版

该版本发布于2019-11-14，sdk c83b3733 1.5.9    

* 增加外部输入扩展 rgba系列 转yuv，外部输出扩展 yuv转rgb
* 修复bug



## 1.5.6版

该版本发布于2019-11-4，sdk 9548693d 1.5.6  

* sdk 修改日志上报无效值 -1
* 过滤无效lostpre 

## 1.5.5版

该版本发布于2019-10-25，sdk dc5b6ccc 1.5.5   

* sdk 修改录像功能，更改sdk文档
* sdk 增加日志上报中pull的streamid 和 userid

## 1.4.0版

该版本发布于2019-10-8，08530576 sdk   

* sdk 修改录像功能
* sdk 增加部分日志上报代码，包括状态信息中的cpu 和 memory ，其余上报日志开关未打开
* sdk 修复切换App 过一阵显示黑屏
* app 配合sdk 修改黑屏，修改录屏兼容性

## 1.3.5版

该版本发布于2019-9-19，e81a594c    

* SDK增加客户端混音功能 支持16位 大小的样本 格式类的音频MP3。
* SDK增加日志上报URL的动态修改

## 1.3.4版

该版本发布于2019-8-23，942b3670    

* SDK增加录像功能，支持640*480,720P,1080P

## 1.2.0版

该版本发布于2019-8-8，e40a90ce    

* 完成新引擎架构调整

## 1.1.1版

该版本发布于2019-7-11，db8ac850    

* 增加大班课小班课功能
* 增加切屏功能
* 修改bug

### 1.0.1版

该版本发布于2019-7-5，c5d06613   

* 修改bug 接收sdp answer streamid写死

## 1.0.1版

该版本发布于2019-7-4。   


* 修改SDK类名前缀统一采用UcloudRtcSdk开头
* 修改SDK Enum变量前缀 UCLOUD_RTC_SDK
* 修改包名 com.ucloudrtclib.sdkengine

## 1.0.0 版本 

该版本发布于2019-7-2。    

* 基本的音视频通话功能	
* 支持内置音视频采集的常见功能	
* 支持静音关闭视频功能	
* 支持视频尺寸的配置(180P - 720P)	
* 支持自动重连	
* 支持丰富的消息回调	
* 支持纯音频互动	
* 支持视频的大小窗口切换	
* 支持获取视频房间统计信息（帧率、码率、丢包率等）	
* 支持编码镜像功能		
* 支持屏幕录制功能
* 支持自动手动订阅 自动手动发布
* 支持权限（上行/下行/全部）控制
* 支持音量提示
* 支持获取sdk版本


# ** iOS **



## 1.5.6版

该版本发布于2020-05-22。  

* 修复视频渲染模式bug
* 添加支持armv7、x86-64架构
* 添加旁路推流
* 添加 网络质量监听


## 1.5.0版

该版本发布于2020-03-25。  

* 优化本地视频录制功能
* 增加播放在线音频的相关回调
* 增加支持应用程序后台模式相关接口

## 1.4版

该版本发布于2020-1-6。  

* 添加本地视频截图功能
* 添加播放在线音频功能，用于背景音效处理


## 1.3版

该版本发布于2019-11-19。  

* 添加流状态监控信息


## 1.2版

该版本发布于2019-8-23。  

* 增加录制功能，最多支持6路混流录制
* 设备测试模块增加视频数据获取功能，方便加入美颜等视频前置处理功能
* 增加媒体自动重连能力，并增加视频数据数据实时回调接口

## 1.1版

该版本发布于2019-7-5。  

* 增加大班课模式
* 增加sdk 模式设置，区分测试 正式模式方便测试
* 优化重连效果


## 1.0版

该版本发布于2019-5-22。      

* 支持1对1 1对多音视频通话  单房间支持最多15人 6人同时连麦
* 支持纯音频通话模式
* 抗丢包 15%  
* 支持配置自动发布模式

# ** macOS **

##  1.1版

该版本发布于2019-7-24。  

1、增加桌面分享功能，并支持指定区域分享    
2、增加sdk 模式设置，区分测试 正式模式方便测试    
3、优化重连效果    

## 1.0 版

该版本发布于2019-7-12。  

1、支持1对1 1对多音视频通话 单房间支持最多15人 6人同时连麦    
2、支持纯音频通话模式    
3、抗丢包 15%    
4、断线制动重连    
5、支持配置自动发布模式   


# ** Electron **

## 1.3版

该版本发布于2019-9-11。   

1、优化抗丢包能力，视频最多抗30%丢包，音频最高抗70%丢包    
2、增加音频文件输入功能，可以替换micphone 输入，支持mp3 wav文件格式 接口    
3、增加音频数据获取功能，支持用户直接获取播放采集音频数据    
4、优化抗丢包能力，30%丢包通话基本连续   
5、增加录制功能，最多支持6路混流录制   
6、设备测试模块增加视频数据获取功能，方便加入美颜等视频前置处理功能   
7、增加媒体自动重连能力，并增加视频数据数据实时回调接口   
8、增加音频数据能量回调功能    

## 1.0版

该版本发布于2019-6-26。    

1、支持1对1 1对多音视频通话  单房间支持最多15人 6人同时连麦    
2、支持纯音频通话模式   
3、抗丢包 15%     
4、断线制动重连   
5、支持配置自动发布模式   




<!-- tabs:end -->
