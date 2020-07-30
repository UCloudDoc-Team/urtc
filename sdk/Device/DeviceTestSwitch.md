# 设备测试与切换

加入房间之前，可以进行设备测试，检测麦克风、摄像头等音视频设备能否正常工作。    

<!-- tabs:start -->

## ** Web **

### 麦克风检测

```js
UCloudRTC.deviceDetection({audio: true}, function(result) {
  if (result.audio) {
    // 设备正常
  } else {
    // 设备异常，异常原因为 result.audioError
  }
});
```

### 摄像头检测

```js
UCloudRTC.deviceDetection({video: true}, function(result) {
  if (result.video) {
    // 设备正常
  } else {
    // 设备异常，异常原因为 result.videoError
  }
});
```

### 同时检测麦克风及摄像头

```js
UCloudRTC.deviceDetection({audio: true, video: true}, function(result) {
  if (result.audio && result.video) {
    // 设备正常
  } else {
    // 设备异常，异常原因为 result.audioError 或 result.videoError
  }
});
```

> 注：指定某设备时，可传入设备的ID，如: `UCloudRTC.deviceDetection({audio: true, microphoneId: 'xxx', video: true, cameraId: 'xxx' }, function(result) {//...}`

### 扬声器检测

由于浏览器的安全限制，Web SDK 不支持直接播放本地音频文件。可以通过以下方法测试音频播放设备：

使用 HTML5 的 <audio> 元素在页面上创建一个音频播放器，播放在线音频文件，并让用户确认是否有声音。
  

## ** Windows **

### 麦克风测试

```cpp
//开启录音测试
//@param  audiolevel 回调实例
virtual int startRecordingDeviceTest(UCloudRtcAudioLevelListener* audiolevel) = 0;

//停止录音测试
virtual int stopRecordingDeviceTest() = 0;

```

### 示例代码

```cpp
auto mediaEngine = UCloudRtcMediaDevice::sharedInstance();
mediaEngine->InitAudioMoudle()
mediaEngine->startRecordingDeviceTest(pAudioListener);  //pAudioListener 继承实现UCloudRtcAudioLevelListener对象的指针
mediaEngine->stopRecordingDeviceTest();
mediaEngine->UnInitAudioMoudle();

```
### 摄像头测试

```cpp
//开启摄像头Capture测试
//@param  profile 分辨率
//@param  observer 回调实例
virtual int startCaptureFrame(eUCloudRtcVideoProfile profile,
UCloudRtcVideoFrameObserver* observer) = 0;

//停止摄像头Capture测试
virtual int stopCaptureFrame() = 0;

```

### 示例代码

```cpp
auto mediaEngine = UCloudRtcMediaDevice::sharedInstance();
mediaEngine->InitAudioMoudle()
mediaEngine->startCaptureFrame(profile,pObserver);  
//profile 抓取的分辨率，pObserver继承实现UCloudRtcVideoFrameObserver的对象指针
mediaEngine->stopCaptureFrame();
mediaEngine->UnInitAudioMoudle();

```

### 扬声器测试

```cpp
//开启播放设备测试
//@param  testAudioFilePath 文件路径
virtual int startPlaybackDeviceTest(const char* testAudioFilePath) = 0;

//停止播放设备测试
virtual int stopPlaybackDeviceTest() = 0;

```

### 示例代码

```cpp
auto mediaEngine = UCloudRtcMediaDevice::sharedInstance();
mediaEngine->InitAudioMoudle()
mediaEngine->startPlaybackDeviceTest(filePath);  //播放测试文件路径
mediaEngine->stopPlaybackDeviceTest();
mediaEngine->UnInitAudioMoudle();

```

<!-- tabs:end -->

## 开发注意事项

所有测试设备的方法都必须在加入房间之前调用。
