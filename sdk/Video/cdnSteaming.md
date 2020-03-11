# 旁路直播

URTC 旁路推流，支持将音视频会议、直播的内容，推流到CDN。用户无需安装APP，可以直接通过Web浏览器观看会议、直播的内容。

在旁路推流过程中，可以把音视频会议、多个主播的音视频混流转码，再推流到CDN。混流会将多个音视频流混合成单个流，并设置音视频参数、混流风格。

点击这里，查看 [旁路推流的计费规则](/video/urtc/price)。

## 1. 功能说明

 - 将会议的主讲人、主播的主播，推流到CDN。 
 - 将参与会议的所有人混流、多个主播混流，推流到CDN。 
 - 支持多种混流风格。 
 - 混流时，支持添加时间水印、文字水印、图片水印。
 
## 2. 前提条件

在实现**旁路推流**之前，请确保已经[集成SDK](/video/urtc/sdk/VideoStart)，在项目中完成基本的音视频会议或者直播功能。

<!-- tabs:start -->

## ** Web **

## 3. 实现方法

### 开启旁路推流

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
    frameRate: 15,
    bitRate: 500
  }
}, (err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
})
```

### 停止旁路推流

```js
client.stopMix({type: 'relay'}, (err, result) => {
  if(err) {
    console.error(err);
    return;
  }
  console.log(result)；
});
```

### 查询旁路推流

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

### 开启旁路推流

``` cpp
tUCloudRtcTranscodeConfig relayconfig;
relayconfig.mbgColor.mBlack = 255;
relayconfig.mbgColor.mGreen = 220; 
relayconfig.mbgColor.mRed = 210;
relayconfig.mBitrate = 500; //
relayconfig.mFramerate = 15;
relayconfig.mWidth = 1280;
relayconfig.mHeight = 720;
relayconfig.mMainviewType = 1;
relayconfig.mMainViewUid = m_userid.data();
m_rtcengine->addPublishStreamUrl("rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll",&relayconfig);
```
   
### 停止旁路推流
``` cpp
m_rtcengine->removePublishStreamUrl("rtmp://publish3.cdn.ucloud.com.cn/ucloud/mylll");
```

### 状态回调
``` cpp
virtual void onRtmpStreamingStateChanged(const int 	state, const char* url, int code);
RTMP_STREAM_PUBLISH_STATE_IDLE , //推流未开始或停止  
RTMP_STREAM_PUBLISH_STATE_RUNNING,  //正在推流
RTMP_STREAM_PUBLISH_STATE_FAILURE , //推流失败 详见code
RTMP_STREAM_PUBLISH_STATE_STOPFAILURE, //停止推流失败 详见code
```


<!-- tabs:end -->

## 5. 开发注意事项

 - 开启旁路推流时，房间内必须有人发布流。
 - 旁路推流时，可以设置推流到Ulive、第三方CDN，然后通过CDN的拉流地址观看。
 
