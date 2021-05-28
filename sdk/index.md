# URTC SDK集成指南
SDK集成指南中，可以了解到Token生成方法，SDK版本说明，集成URTC SDK步骤，使用屏幕分享、播放混音、旁路推流的方法。

## [URTC常用术语](urtc/sdk/term)
介绍URTC产品使用中常用的术语概念。

## [Token生成指导](urtc/sdk/token)
`Token`是URTC验证客户身份的关键信息，通过`Appid`、`Appkey`生成。 通过[Token生成指导](urtc/sdk/token)，可以获取生成`Token`示例代码。    
在测试阶段，可以通过客户端生成`Token` ，以便尽快开始集成测试。        
在产环境中，建议在后台服务器中部署`Token`服务。客户端加入房间之前，向后台服务器申请`Token`，以保证`Token`的安全性。     

## [SDK版本说明](urtc/sdk/Version)
[SDK版本说明](/urtc/sdk/Version)中，可以了解到不同版本的功能差异和版本发布日期。

## [快速集成SDK](urtc/sdk/VideoStart)
通过[快速集成SDK](urtc/sdk/VideoStart)，可以快速集成URTC SDK，了解创建音视频通话、互动直播的基本步骤。    
也可以通过各个客户端的demo和SDK API接口文档，了解更多功能。

## 常用功能
了解常用功能的SDK API调用方法：    
* [互动连麦](/urtc/sdk/Video/Interactive)        
* [屏幕共享](/urtc/sdk/Video/screenshare)     
* [关闭音视频](/urtc/sdk/Video/mute)    
* [设置视频参数](/urtc/sdk/Video/videoProfile)    
* [调整音量](/urtc/sdk/Audio/AudioVolume)   		
* [播放混音](/urtc/sdk/Audio/AudioMixing)   	
* [视频快照](/urtc/sdk/Video/videoSnap)   
* [视频采集旋转](urtc/sdk/Video/VideoRotation)   
* [自定义视频采集](/urtc/sdk/Video/CustomVideoInput)  
* [云代理服务](/urtc/sdk/Video/proxy)     		
* [设备测试与切换](/urtc/sdk/Device/DeviceTestSwitch)     
* [通话前检测网络质量](/urtc/sdk/Device/TestQuality)   
* [通话中质量监测](/urtc/sdk/Device/CallQuality)   
* [浏览器的自动播放策略](/urtc/sdk/Video/WebAutoPlay)  
* [断网自动重连策略](/urtc/sdk/Device/ReConnect)  
