# 旁路直播

URTC 旁路推流，支持将音视频会议、直播的内容，推流到CDN。用户无需安装APP，可以直接通过Web浏览器观看会议、直播的内容。

在旁路推流过程中，可以把音视频会议、多个主播的音视频混流转码，再推流到CDN。混流会将多个音视频流混合成单个流，并设置音视频参数、混流风格。

点击这里，查看 [旁路推流的计费规则](/video/urtc/price)。

## 1、功能说明

 - 将会议的主讲人、主播的主播，推流到CDN。 
 - 将参与会议的所有人混流、多个主播混流，推流到CDN。 
 - 支持多种混流风格。 
 - 混流时，支持添加时间水印、文字水印、图片水印。
 
## 2、前提条件

在实现**旁路推流**之前，请确保已经[集成SDK](/video/urtc/sdk/VideoStart)，在项目中完成基本的音视频会议或者直播功能。

## 3、实现方法

参考如下步骤，在你的项目中实现**旁路推流**：

以下为声网的:

调用 Stream.init 成功后，频道内主播可以调用 Client.setLiveTranscoding 方法设置音视频流的直播参数 （LiveTranscoding），如分辨率、码率、帧率、背景色和水印位置。如果你需要转码合图，请在 TranscodingUser 类中设置每个用户的视频参数，详见示例代码。

如果直播参数（LiveTranscoding）有更新，Client.on("liveTranscodingUpdated") 回调会被触发并向主播报告更新信息。

频道内主播可以调用 Client.startLiveStreaming 方法向 CDN 推流直播中增加指定的一路媒体流。推流地址可以在推流后动态增删。

请通过 enableTranscoding 设置是否转码推流。

频道内主播可以调用 Client.stopLiveStreaming 方法向 CDN 推流直播中删除指定的一路媒体流。

增加或删除一路媒体流时，SDK 会触发 Client.on(liveStreamingStarted) 或 Client.on(liveStreamingFailed)回调向主播报告当前推流状态。请确保收到该回调后再调用 API 进行下一步操作。


## 4、示例代码

<!-- tabs:start -->

### ** Web **

Web示例代码：

啦啦啦啦


### ** Windows **

Windows示例代码：

啦啦啦啦



### ** Android **

Android示例代码：

啦啦啦啦



### ** iOS **

iOS示例代码：

啦啦啦啦

<!-- tabs:end -->

## 5、开发注意事项

 - URTC
