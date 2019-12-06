# 质量监控

URTC服务支持监控实时通话质量，在控制台 [实时音视频URTC](https://console.ucloud.cn/urtc/manage) 里，查看我的应用中的质量监控。    
如果想在自己的服务器内集成实时音视频通话的监控质量，可以集成 [质量监控API](https://docs.ucloud.cn/api/urtc-api/overview) 。


功能：  
  - 房间监控：监控实时或者历史的房间信息
  - 用户监控：监控某房间的用户信息
  - 流监控：监控某路流的信息
  
## 1、房间监控

支持监控实时或者历史的所有房间列表、房间内累计用户数、房间内峰值用户数、房间内在线用户数。

  ![ ](/images/qualityImage/roomnew.png)
 
 
## 2、用户监控
  
支持监控某房间的所有用户列表、所有用户通话时长、设备可用状态、设备型号、SDK版本、网络类型。    
  
  ![ ](/images/qualityImage/usersnew.png)


  - 区域：用户所属的区域。  
  - 持续时间：用户加入房间的时间、退出房间的时间。  
  - 通话状态：用户加入房间的时间为蓝色，相对房间存在的时间为灰色。  
  - 设备：麦克风/扬声器/摄像头都可用，则状态为可用；有设备不可用，则显示有异常；具体异常情况，可以查看 质量监控中的事件。  
  - 设备型号：用户终端的型号及系统版本；如果是WEB端，还会显示浏览器型号及版本。  
  - SDK版本：终端SDK的版本。  
  - 网络类型：当前终端的网络：LAN、WIFI、4G、3G、2G、NA（web)。
  
> WEB SDK受限于浏览器权限，无法获得电脑或者手机的网络类型，控制台上用 `NA` 标识。

  - 支持监控所有用户的所有事件、上下行码率、卡顿情况。  

  ![ ](/images/qualityImage/userqualitynew.png)
  
  - 上行码率：当前用户发送的音视频的总码率；码率稳定表示终端编码、上行网络稳定。
  - 下行码率：当前用户接收的音视频的总码率；码率稳定表示下行网络稳定。
  - 卡顿：当前用户的视频卡顿情况；没有卡顿表示用户接收的音视频流畅，有卡顿表示用户接收的音视频不流畅。
	 
## 3、流监控

支持监控某路流的上下行终端信息、上下行音频码率/视频码率、上下行丢包率、采集/播放音量、发送/接收延迟、上下行帧率、APP CPU占用率、APP内存使用数。   

  ![ ](/images/qualityImage/userquality2new.png)

  - 上行音频码率：当前用户发送的音频码率；码率稳定表示终端音频编码、上行网络稳定。
  - 下行音频码率：当前用户接收的音频码率；码率稳定表示下行网络稳定。
  
  
  - 上行视频码率：当前用户发送的视频码率；码率稳定表示终端音频编码、上行网络稳定。
  - 下行视频码率：当前用户接收的视频码率；码率稳定表示下行网络稳定。
  
  
  - 上行丢包率：当前用户发送的丢包率；丢包率低表示音视频上行网络稳定。
  - 下行丢包率：当前用户接收的丢包率；丢包率低表示音视频下行网络稳定。
  
  
  - 采集音量：当前用户发送的丢包率；丢包率低表示音视频上行网络稳定。
  - 播放音量：当前用户接收的丢包率；丢包率低表示音视频下行网络稳定。
  
  
  - 发送延迟：当前用户发送的延迟时间；延迟低表示音视频上行网络性能较高。  
  - 接收延迟：当前用户发送的延迟时间；延迟低表示音视频上行网络性能较高。	 
  
  
  - 上行帧率：当前用户发送视频的帧率；帧率稳定表示音视频编码、上行网络稳定。
  - 下行帧率：当前用户接收视频的帧率；帧率稳定表示音视频解码、下行网络稳定。
      
      
  - CPU占用率：当前应用在终端上的CPU占用率；CPU占用率低表示终端性能充足。
  - 内存使用数：当前应用在终端上的内存使用数；内存使用数低且稳定表示终端性能充足。