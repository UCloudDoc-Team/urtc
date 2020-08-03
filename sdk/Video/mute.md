# 关闭音视频

在通话中，在不取消发布、订阅的情况下，可以通过API关闭声音、关闭视频，暂时不发音视频、或者暂时不收音视频。

- 关闭本端声音、视频：关闭后，其他客户端无法听到本端的声音、看到本端的视频。
- 关闭远端声音、视频：关闭后，本端不听、不看；并不影响其他客户端的接收。

<!-- tabs:start -->

## ** Web **

### 实现方法

Web API|方法说明
:-: |:-: 
muteAudio 方法	 | 关闭麦克风，可以关闭本端、远端音频，根据StreamId设置
unmuteAudio 方法	 | 打开麦克风，可以打开本端、远端音频，根据StreamId设置
muteVideo 方法	 | 关闭视频，可以关闭本端、远端视频，根据StreamId设置
unmuteVideo 方法	 | 打开视频，可以打开本端、远端视频，根据StreamId设置

### 示例代码

```
this.client.muteAudio(stream.sid);  // 关闭本地、远端音频
this.client.unmuteAudio(stream.sid);  // 取消关闭本地、远端音频
this.client.muteVideo(stream.sid);  // 关闭本地、远端视频
this.client.unmuteVideo(stream.sid);  // 取消关闭本地、远端视频
```

## ** Windows **

### 实现方法

Windows API|方法说明
:-: |:-: 
muteLocalMic 方法	 | 打开/关闭本地麦克风
muteLocalVideo 方法	 | 打开/关闭本地视频
muteRemoteAudio 方法	 | 打开/关闭远端音频
muteRemoteVideo 方法	 | 打开/关闭远端视频

### 示例代码

文档待更新

## ** Android **

### 实现方法

Android API|方法说明
:-: |:-: 
muteLocalMic 方法	 | 打开/关闭本地麦克风
muteLocalVideo 方法	 | 打开/关闭本地视频
muteRemoteAudio 方法	 | 打开/关闭远端音频
muteRemoteVideo 方法	 | 打开/关闭远端视频
muteRemoteScreen 方法	 | 打开/关闭远端桌面

### 示例代码

```java
//关闭本地麦克风
sdkEngine.muteLocalMic(true);
//关闭本地屏幕流
sdkEngine.muteLocalVideo(true, UCLOUD_RTC_SDK_MEDIA_TYPE_SCREEN);
//关闭本地视频流
sdkEngine.muteLocalVideo(true, UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO);
//关闭远端音频
sdkEngine.muteLocalVideo(userId, true);
//关闭远端视频流
sdkEngine.muteRemoteVideo(userId, true);
//关闭远端桌面流
sdkEngine.muteRemoteScreen(userId, true);

```

## ** iOS **

### 实现方法

iOS API|方法说明
:-: |:-: 
setMute 方法	 | 打开/关闭本地麦克风
openCamera 方法	 | 打开/关闭本地视频
setRemoteStream:muteVideo 方法	 | 打开/关闭远端音频
setRemoteStream:muteAudio 方法	 | 打开/关闭远端视频

### 示例代码

文档待更新


<!-- tabs:end -->
