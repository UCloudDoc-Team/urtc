# SDK集成指南

SDK集成指南中，可以了解到Token生成方法，SDK版本说明，集成URTC SDK步骤，使用屏幕分享、播放混音、旁路推的方法。

* [Token生成指导](urtc/sdk/token)
* [SDK版本说明](urtc/sdk/Version)
* [快速集成SDK](urtc/sdk/VideoStart)
* [屏幕共享](urtc/sdk/Video/screenshare)
* [播放混音](urtc/sdk/Audio/AudioMixing)
* [旁路推流](urtc/sdk/Video/cdnSteaming)

## Token生成指导

`Token`是URTC验证客户身份的关键信息，通过`Appid`、`Appkey`生成。 通过[Token生成指导](urtc/sdk/token)，可以获取生成`Token`示例代码。    
在测试阶段，可以通过客户端生成`Token` ，以便尽快开始集成测试。        
在产环境中，建议在后台服务器中部署`Token`服务。客户端加入房间之前，向后台服务器申请`Token`，以保证`Token`的安全性。     
 
## SDK版本说明
 
[SDK版本说明](/urtc/sdk/Version)中，可以了解到不同版本的功能差异和版本发布日期。
 
## 快速集成SDK
 
 通过[快速集成SDK](urtc/sdk/VideoStart)，可以快速集成URTC SDK，了解创建音视频通话、互动直播的基本步骤。    
 也可以通过各个客户端的demo和SDK API接口文档，了解更多功能。
 
## 屏幕共享

通过[屏幕共享](urtc/sdk/Video/screenshare)，可以把客户端的屏幕，以视频的方式分享给房间内的其他人。    
因为浏览器对于获取屏幕分享的限制，这里重点介绍了WEB客户端的屏幕共享代码方法。    
Windows、安卓客户端的屏幕共享，已经在SDK里直接集成。具体的API接口，参照[快速集成SDK](urtc/sdk/VideoStart)中SDK API接口文档。
 
## 播放混音

通过[播放混音](urtc/sdk/Audio/AudioMixing)，可以播放自定义的音源、音乐文件让房间内的其他用户听到，比如播放背景音乐等。

## 旁路推流

通过[旁路推流](urtc/sdk/Video/cdnSteaming)，可以将音视频会议、直播的内容，推流到CDN。用户无需安装APP，可以直接通过Web浏览器观看会议、直播的内容。
 
