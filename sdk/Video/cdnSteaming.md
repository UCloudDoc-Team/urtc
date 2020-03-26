# 旁路直播

URTC 旁路推流，支持将音视频会议、直播的内容，推流到CDN。用户无需安装APP，可以直接通过Web浏览器观看会议、直播的内容。

在旁路推流过程中，可以把音视频会议、多个主播的音视频混流转码，再推流到CDN。混流会将多个音视频流混合成单个流，并设置音视频参数、混流风格。

点击这里，查看 [旁路推流的计费规则](urtc/price)。

## 1. 功能说明

 - 将会议的主讲人、主播的主播，推流到CDN。 
 - 将参与会议的所有人混流、多个主播混流，推流到CDN。 
 - 支持多种混流风格。 
 - 混流时，支持添加时间水印、文字水印、图片水印。
 
## 2. 前提条件

在实现**旁路推流**之前，请确保已经[集成SDK](urtc/sdk/VideoStart)，在项目中完成基本的音视频会议或者直播功能。

## 3. 实现方法

<!-- tabs:start -->

## ** Web **

### Web开启旁路推流

```js
client.startMix({
  type: 'relay',
  pushURL: ['rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll'],
  layout: {
    type: 'main',
    mainViewUid: 'user-xxx',
    mainViewType: 'camera'
  },
  video: {
    codec: 'h264',
    quality: 'CB',
    frameRate: 15,
    bitRate: 500
  },
  width: 1280,
  height: 720,
  backgroundColor: {
    r: 0,
    g: 0,
    b: 0
  }
}, (err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
})

```

### Web停止旁路推流

```js
client.stopMix({type: 'relay'}, (err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
});

```

### Web查询旁路推流

```js
client.queryMix((err, result) => {
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
relayconfig.mBitrate = 500;
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
//[混流风格](urtc/cloudRecord/RecordLaylout)（1.平铺 2.垂直风格(大小布局) 3.自定义（指定style）4.模板自适应一 5. 模板自适应二 ）
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
relayconfig.mBitrate = 500;
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
//[混流风格](urtc/cloudRecord/RecordLaylout)（1.平铺 2.垂直风格(大小布局) 3.自定义（指定style）4.模板自适应一 5. 模板自适应二 ）
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


<!-- tabs:end -->

## 4. 开发注意事项

 - 开启旁路推流时，房间内必须有人发布流。
 - 旁路推流时，可以设置推流到Ulive、第三方CDN，然后通过拉流地址观看。
