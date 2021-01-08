# 旁路直播

URTC 旁路推流，支持将音视频会议、直播的内容，推流到直播CDN。用户无需安装APP，可以直接通过Web浏览器拉CDN直播流，观看会议、直播的内容。

![](/images/sdk/Video/cdnSteaming.png)

在旁路推流过程中，可以推单路流到直播CDN；也可以把音视频会议、多个主播的音视频混流转码，再推流到直播CDN。混流会将多个音视频流混合成单个流，并设置音视频参数、混流风格。

点击这里，查看 [旁路推流的计费规则](urtc/price)。

## 1. 功能说明

 - 将会议的主讲人、主播的主播，推流到直播CDN。 
 - 将参与会议的所有人混流、多个主播混流，推流到直播CDN。 
 - 支持多种混流风格。 
 - 混流时，支持添加时间水印、文字水印、图片水印。
 
## 2. 前提条件

在实现**旁路推流**之前，请确保已经[集成SDK](urtc/sdk/VideoStart)，在项目中完成基本的音视频会议或者直播功能。

## 3. 实现方法

<!-- tabs:start -->

## ** Web **

### Web开启旁路推流

```js
client.startRelay({
  pushURL: ['rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll'],
  layout: {
    type: 'main',
    mainViewUid: 'user-xxx',
    mainViewType: 'camera'
  },
  width: 1280,
  height: 720,
}, (err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
})

```

注：此处仅列出了部分参数，更多参数请查看 [sdk 中 API 文档中的相关说明](https://github.com/ucloud/urtc-sdk-web#client-startrelay)

### Web停止旁路推流

```js
client.stopRelay((err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
});

```

## ** Windows **

### Windows开启旁路推流

```cpp
tUCloudRtcTranscodeConfig relayconfig;
//转推配置
relayconfig.mbgColor.mRed = 0;
relayconfig.mbgColor.mGreen = 0; 
relayconfig.mbgColor.mBlue = 0;
//转推背景色
relayconfig.mBitrate = 1000;
//码率
relayconfig.mFramerate = 15;
//帧率
relayconfig.mWidth = 1280;
//输出分辨率宽度
relayconfig.mHeight = 720;
//输出分辨率高度
relayconfig.mMainviewType = 1;
//主流的媒体类型，1是摄像头，2是桌面
relayconfig.mMainViewUid = m_userid.data();
//主流用户ID
relayconfig.mStyle = nullptr;
//自定义风格
relayconfig.mLayout = 1;
//混流风格（1.平铺 2.垂直风格(大小布局) 3.自定义（指定style）4.模板自适应一 5. 模板自适应二 ）
relayconfig.mStreams = nullptr;
//混流的用户(默认混房间内全部流)
relayconfig.mStreamslength = 0;
//混流的用户人数，0是根据房间推流自动混流，1~16是设定的混流数量
m_rtcengine->addPublishStreamUrl("rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll",&relayconfig);
//旁路推流的CDN地址

```

旁路推流时，除了默认混流风格，还可以自定义推流风格。    
自定义布局填在custom里，格式参照RFC5707 Media Server Markup Language (MSML)。   
以下是1画面全屏，2画面均分的示例：    


```cpp
//转推配置
tUCloudRtcTranscodeConfig relayconfig;
//转推背景色
relayconfig.mbgColor.mRed = 0;
relayconfig.mbgColor.mGreen = 0; 
relayconfig.mbgColor.mBlue = 0;
//码率
relayconfig.mBitrate = 1000;
//帧率
relayconfig.mFramerate = 15;
//输出分辨率宽度
relayconfig.mWidth = 1280;
//输出分辨率高度
relayconfig.mHeight = 720;
//主流的媒体类型，1是摄像头，2是桌面
relayconfig.mMainviewType = 1;
//主流用户ID
relayconfig.mMainViewUid = m_userid.data();
//自定义风格
relayconfig.mStyle = nullptr;
//混流风格（1.平铺 2.垂直风格(大小布局) 3.自定义（指定style）4.模板自适应一 5. 模板自适应二 ）
relayconfig.mLayout = 3;   //设置自定义风格
//混流的用户(默认混房间内全部流)
relayconfig.mStreams = nullptr;
//混流的用户人数，0是根据房间推流自动混流；支持设置1~16路视频混流
relayconfig.mStreamslength = 0;
//自定义风格串
relayconfig.mStyle ="{\"custom\":[{\"region\":[{\"id\":\"1\",\"shape\":\"rectangle\",\"area\":{\"left\":\"0\",\"top\":\"0\",\"width\":\"1\",\"height\":\"1\"}}]},{\"region\":[{\"id\":\"1\",\"shape\":\"rectangle\",\"area\":{\"left\":\"0\",\"top\":\"1/4\",\"width\":\"1/2\",\"height\":\"1/2\"}},{\"id\":\"2\",\"shape\":\"rectangle\",\"area\":{\"left\":\"1/2\",\"top\":\"1/4\",\"width\":\"1/2\",\"height\":\"1/2\"}}]}]}";
m_rtcengine->addPublishStreamUrl("rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll",&relayconfig);

```

   
### Windows停止旁路推流

```cpp
m_rtcengine->removePublishStreamUrl("rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll");
```

### Windows状态回调

```cpp
virtual void onRtmpStreamingStateChanged(const int 	state, const char* url, int code);
RTMP_STREAM_PUBLISH_STATE_IDLE
//推流未开始或停止  
RTMP_STREAM_PUBLISH_STATE_RUNNING
//正在推流
RTMP_STREAM_PUBLISH_STATE_FAILURE
//推流失败 详见code
RTMP_STREAM_PUBLISH_STATE_STOPFAILURE
//停止推流失败 详见code
RTMP_STREAM_PUBLISH_STATE_EXCEPTIONSTOP 
//异常停止推流
```

## ** Android **

### Android开启旁路推流

```java
UCloudRtcSdkMixProfile mixProfile = UCloudRtcSdkMixProfile.getInstance().assembleMixParamsBuilder()
        .type(UCloudRtcSdkMixProfile.MIX_TYPE_RELAY)
        //画面模式
        .layout(UCloudRtcSdkMixProfile.LAYOUT_CLASS_ROOM_2)
        //画面分辨率
        .resolution(1280, 720)
        //背景色
        .bgColor(0, 0, 0)
        //画面帧率
        .frameRate(15)
        //画面码率
        .bitRate(1000)
        //h264视频编码
        .videoCodec(UCloudRtcSdkMixProfile.VIDEO_CODEC_H264)
        //编码质量
        .qualityLevel(UCloudRtcSdkMixProfile.QUALITY_H264_CB)
        //音频编码
        .audioCodec(UCloudRtcSdkMixProfile.AUDIO_CODEC_AAC)
        //主讲人ID
        .mainViewUserId(mUserid)
        //主讲人媒体类型
        .mainViewMediaType(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
        //加流方式手动
        .addStreamMode(UCloudRtcSdkMixProfile.ADD_STREAM_MODE_MANUAL)
        //添加流列表，也可以后续调用MIX_TYPE_UPDATE 动态添加
        .addStream(mUserid,UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
        //设置转推cdn 的地址
        .addPushUrl("rtmp://rtcpush.ugslb.com/rtclive/" + mRoomid)
        .build();
sdkEngine.startRelay(mixProfile);
```
### Android停止旁路推流

```java
//类型是转推+录制,地址留空停止对所有url的转推
sdkEngine.stopRelay(null); 
```
### Android添加混流

```java

UCloudRtcSdkMixProfile mixProfile = UCloudRtcSdkMixProfile.getInstance().assembleMixParamsBuilder()
        //更新混流状态
        .type(UCloudRtcSdkMixProfile.MIX_TYPE_UPDATE)
        //画面模式
        .layout(UCloudRtcSdkMixProfile.LAYOUT_CLASS_ROOM_2)
        //画面分辨率
        .resolution(1280, 720)
        //背景色
        .bgColor(0, 0, 0)
        //画面帧率
        .frameRate(15)
        //画面码率
        .bitRate(1000)
        //h264视频编码
        .videoCodec(UCloudRtcSdkMixProfile.VIDEO_CODEC_H264)
        //编码质量
        .qualityLevel(UCloudRtcSdkMixProfile.QUALITY_H264_CB)
        //音频编码
        .audioCodec(UCloudRtcSdkMixProfile.AUDIO_CODEC_AAC)
        //主讲人ID
        .mainViewUserId(mUserid)
        //主讲人媒体类型
        .mainViewMediaType(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
        //加流方式手动
        .addStreamMode(UCloudRtcSdkMixProfile.ADD_STREAM_MODE_MANUAL)
        //添加流列表，参数一用户id，参数二推流来源（屏幕或camera）
        .addStream("testId",UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
        //设置转推cdn 的地址
        .addPushUrl("rtmp://rtcpush.ugslb.com/rtclive/" + mRoomid)
        .build();
sdkEngine.updateMixConfig(mixProfile);
```


## ** iOS **

### iOS开启旁路推流

```swift
let mixConfig = UCloudRtcMixConfig.init();
mixConfig.type = 1; //1 旁路推流
mixConfig.streams = @[]; //如果指定了用户，则只添加该用户的指定流，新加入的流处理由addstreammode参数决定
mixConfig.pushurl = @[@"rtmp://rtcpush.ugslb.com/rtclive/URtc-h4r1txxy12111151yketwz111"]; //转推地址
mixConfig.layout = 1; //1 流式(均分)布局, 2 讲课模式，主讲人占大部分屏幕，其他人小屏居于右侧或底部 3 自定义布局 4 定制讲课模式 5 定制均分模式
mixConfig.layouts = @[]; //可选多布局
mixConfig.bgColor = @{@"r": @200,@"g": @100, @"b": @50}; //背景色
mixConfig.bitrate = 1000; //比特率
mixConfig.framerate = 15; //画面帧率
mixConfig.videocodec = @"H264"; //视频编码
mixConfig.qualitylevel = @"CB"; //编码质量
mixConfig.audiocodec = @"aac"; //aac音频编码
mixConfig.mainviewtype = 1; //主讲人id
mixConfig.width = 720; //画面分辨率 宽
mixConfig.height = 1280; //画面分辨率 高
//开始旁路推流
self.manager?.startMix(mixConfig);
```

```objectivec
UCloudRtcMixConfig *mixConfig = [UCloudRtcMixConfig new];
mixConfig.type = 1; //1 旁路推流
mixConfig.streams = @[]; //如果指定了用户，则只添加该用户的指定流，新加入的流处理由addstreammode参数决定
mixConfig.pushurl = @[@"rtmp://rtcpush.ugslb.com/rtclive/URtc-h4r1txxy12111151yketwz111"]; //转推地址
mixConfig.layout = 1; //1 流式(均分)布局, 2 讲课模式，主讲人占大部分屏幕，其他人小屏居于右侧或底部 3 自定义布局 4 定制讲课模式 5 定制均分模式
mixConfig.layouts = @[]; //可选多布局
mixConfig.bgColor = @{@"r": @200,@"g": @100, @"b": @50}; //背景色
mixConfig.bitrate = 1000; //比特率
mixConfig.framerate = 15; //画面帧率
mixConfig.videocodec = @"H264"; //视频编码
mixConfig.qualitylevel = @"CB"; //编码质量
mixConfig.audiocodec = @"aac"; //aac音频编码
mixConfig.mainviewtype = 1; //主讲人id
mixConfig.width = 720; //画面分辨率 宽
mixConfig.height = 1280; //画面分辨率 高
//开始旁路推流
[self.manager startMix:mixConfig];
```
### iOS停止旁路推流

```swift
//地址留空停止对所有url的转推
let mixStopConfig = UCloudRtcMixStopConfig.init()
mixStopConfig.type = 1;
mixStopConfig.pushurl = @[@""];
self.manager?.stopMix(mixstopConfig)
```

```objectivec
UCloudRtcMixStopConfig *mixStopConfig = [UCloudRtcMixStopConfig new];
mixStopConfig.type = 1;
mixStopConfig.pushurl = @[@""];
[self.manager stopMix: mixStopConfig];
```
### iOS添加混流
```swift
//streams [{"user_id": "","media_type": 1 //1 摄像头  2 桌面}]
self.manager?.addMixStream(streams)
```

```objectivec
//streams [{"user_id": "","media_type": 1 //1 摄像头  2 桌面}]
[self.manager addMixStream:streams];
```
### iOS删除混流

```swift
//streams [{"user_id": "","media_type": 1 //1 摄像头  2 桌面}]
self.manager?.deleteMixStream(streams);
```

```objectivec
//streams [{"user_id": "","media_type": 1 //1 摄像头  2 桌面}]
[self.manager deleteMixStream:streams];
```

<!-- tabs:end -->

## 4. 播放直播流

不同直播服务平台的直播流域名规则，不完全相同，这里只以UCloud的 [直播云ULive](https://docs.ucloud.cn/ulive/README) 举例。    

示例： 推流域名：publish.company.com，接入点：test。rtmp播放域名：rtmp.company.com， hls播放域名：hls.company.com 。    
则    
推流地址为：rtmp://publish.company.com/test/{Roomid}       
rtmp播放地址为：rtmp://rtmp.company.com/test/{Roomid}    
hls播放地址为：http://hls.company.com/test/{Roomid}/playlist.m3u8    

> 播放直播流产生的流量，按照 [直播云ULive产品价格](https://docs.ucloud.cn/ulive/charge) 而定。

## 5. 开发注意事项

 - 开启旁路推流时，房间内必须有人发布流。
 - 旁路推流，混流时的合成风格，可以参考 [混流风格](urtc/cloudRecord/RecordLaylout)。
 - 旁路推流时，可以设置推流到直播云ULive、第三方CDN，然后通过拉流地址观看。
