# 旁路推流

URTC 旁路推流，支持将音视频会议、直播的内容，推流到直播CDN。用户无需安装APP，可以直接通过Web浏览器拉CDN直播流，观看会议、直播的内容。

![](/images/cdnSteamingImage/cdnSteamingV2.png)

在旁路推流过程中，可以推单路流到直播CDN；也可以把音视频会议、多个主播的音视频混流转码，再推流到直播CDN。混流会将多个音视频流混合成单个流，并设置音视频参数、混流风格。

点击这里，查看 [旁路推流的计费规则](urtc/price)。

## 功能说明

 - 将会议的主讲人、主播的主播，推流到直播CDN。 
 - 将参与会议的所有人混流、多个主播混流，推流到直播CDN。 
 - 支持多种混流风格。 
 - 混流时，支持添加时间水印、文字水印、图片水印。

## 使用方式

 - 客户端 可以通过[SDK直接调用API](urtc/cdnSteaming/cdnSteaming_SDK)来实现 旁路推流。
 - 服务端 可以通过[RESTful API](urtc/cdnSteaming/cdnSteaming_RESTful)来实现 旁路推流。
