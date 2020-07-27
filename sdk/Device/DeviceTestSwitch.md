# 设备测试与切换

加入房间之前，可以进行设备测试，检测麦克风、摄像头等音视频设备能否正常工作。    


<!-- tabs:start -->

## ** Web **

### 麦克风测试

balabala……    

### 示例代码

balabala……   

### 扬声器测试

balabala……    

### 示例代码

balabala……   

### 摄像头测试

balabala……    

### 示例代码

balabala……  


## ** Windows **

### 麦克风测试

```cpp
	///开启录音测试
	///@param  audiolevel 回调实例
	virtual int startRecordingDeviceTest(UCloudRtcAudioLevelListener* audiolevel) = 0;

	///停止录音测试
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
### 扬声器测试

```cpp
	///开启播放设备测试
	///@param  testAudioFilePath 文件路径
	virtual int startPlaybackDeviceTest(const char* testAudioFilePath) = 0;

	///停止播放设备测试
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

### 摄像头测试

```cpp
	///开启摄像头Capture测试
	///@param  profile 分辨率
	///@param  observer 回调实例
	virtual int startCaptureFrame(eUCloudRtcVideoProfile profile,
		UCloudRtcVideoFrameObserver* observer) = 0;

	///停止摄像头Capture测试
	virtual int stopCaptureFrame() = 0;
```

### 示例代码

```cpp
auto mediaEngine = UCloudRtcMediaDevice::sharedInstance();
mediaEngine->InitAudioMoudle()
mediaEngine->startCaptureFrame(profile,pObserver);  //profile 抓取的分辨率，pObserver继承实现UCloudRtcVideoFrameObserver的对象指针
mediaEngine->stopCaptureFrame();
mediaEngine->UnInitAudioMoudle();

```

# ** Android **

### 麦克风测试

balabala……    

### 示例代码

balabala……   

### 扬声器测试

balabala……    

### 示例代码

balabala……   

### 摄像头测试

balabala……    

### 示例代码

balabala……  

# ** iOS **

### 麦克风测试

balabala……    

### 示例代码

balabala……   

### 扬声器测试

balabala……    

### 示例代码

balabala……   

### 摄像头测试

balabala……    

### 示例代码

balabala……  

<!-- tabs:end -->

## 开发注意事项

所有测试设备的方法都必须在加入房间之前调用。
