# 质量监控

URTC服务支持监控实时通话质量，在控制台 [实时音视频URTC](https://console.ucloud.cn/urtc/manage) 里，查看我的应用中的质量监控。    
如果想在自己的服务器内集成实时音视频通话的监控质量，可以集成 [质量监控API](https://docs.ucloud.cn/api/urtc-api/overview) 。


功能：  
  - 房间监控：监控实时或者历史的房间信息
  - 用户监控：监控某房间的用户信息
  - 流监控：监控某路流的信息
  
## 1、房间监控

  - 支持监控实时或者历史的所有房间列表、房间内累计用户数、房间内峰值用户数、房间内在线用户数。

  ![ ](/images/qualityImage/roomnew.png)
  
## 2、用户监控
  
  - 支持监控某房间的所有用户列表、所有用户通话时长、麦克风/扬声器/摄像头的设备可用状态、设备型号、SDK版本、网络类型。    
  
  ![ ](/images/qualityImage/usersnew.png)


> WEB SDK受限于浏览器权限，无法获得电脑或者手机的网络类型，控制台上用 `NA` 标识。

  - 支持监控所有用户的所有事件、上下行码率、卡顿情况。  

  ![ ](/images/qualityImage/userqualitynew.png)
	 
## 3、流监控

  - 支持监控某路流的上下行终端信息、上下行音频码率/视频码率、上下行丢包率、采集/播放音量、发送/接收延迟、上下行帧率、APP CPU占用率、APP内存使用数。   

  ![ ](/images/qualityImage/userquality2new.png)



	 
  
  
