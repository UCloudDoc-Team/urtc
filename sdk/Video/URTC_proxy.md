# 云代理服务

有防火墙限制、无法直接连接互联网的环境，，如银行、学校、医院、企业内网等，使用URTC时，可以使用 *云代理服务*。    
内网客户端 通过 *云代理* 连接 互联网的客户端。  


<!-- {docsify-ignore-all} -->

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

```js
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

```cpp
//关闭视频流本地麦克风
sdkEngine.muteLocalMic(true,UCLOUD_RTC_MEDIATYPE_VIDEO);

//关闭桌面流本地麦克风
sdkEngine.muteLocalMic(true,UCLOUD_RTC_MEDIATYPE_SCREEN);

//关闭本地屏幕流
sdkEngine.muteLocalVideo(true, UCLOUD_RTC_MEDIATYPE_SCREEN);

//关闭本地视频流
sdkEngine.muteLocalVideo(true, UCLOUD_RTC_MEDIATYPE_VIDEO);

//关闭远端视频音频
tUCloudRtcMuteSt romoteUser;
romoteUser.mUserId = "xxxx";
romoteUser.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;
sdkEngine.muteRemoteAudio(userId, true);

//关闭远端桌面音频
tUCloudRtcMuteSt romoteUser;
romoteUser.mUserId = "xxxx";
romoteUser.mStreamMtype = UCLOUD_RTC_MEDIATYPE_SCREEN;
sdkEngine.muteRemoteAudio(userId, true);

//关闭远端视频流
tUCloudRtcMuteSt romoteUser;
romoteUser.mUserId = "xxxx";
romoteUser.mStreamMtype = UCLOUD_RTC_MEDIATYPE_VIDEO;
sdkEngine.muteRemoteVideo(userId, true);

//关闭远端桌面流
tUCloudRtcMuteSt romoteUser;
romoteUser.mUserId = "xxxx";
romoteUser.mStreamMtype = UCLOUD_RTC_MEDIATYPE_SCREEN;
sdkEngine.muteRemoteScreen(userId, true);

```

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
```objectivec
//关闭本地麦克风
[rtcEngine setMute:YES];

//关闭本地视频流
[rtcEngine openCamera:YES];

//关闭远端音频
[rtcEngine setRemoteStream:stream muteAudio:YES];

//关闭远端视频流
[rtcEngine setRemoteStream:stream muteVideo:YES];

```


<!-- tabs:end -->
