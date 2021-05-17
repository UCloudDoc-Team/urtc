# 互动连麦

在大班课、互动直播的过程中，学生、观众可以连麦，与老师、主播进行互动，丰富课堂、直播体验，提升教学、直播效果。
<!-- {docsify-ignore-all} -->
<!-- tabs:start -->

## ** Web **

### 开始连麦

- 在使用用户权限`role`为 `pull` 的情况下，开始连麦时，要更改用户权限`role`为`push-and-pull`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```js
const result = client.setRole('push-and-pull')//result: boolean 类型，成功时为 true，失败时为 false
if(result){
    client.publish({
      audio: true/false
      // 必填，指定是否使用麦克风设备
      video: true/false
      // 必填，指定是否使用摄像头设备
      screen: true/false
      // 必填，指定是否为屏幕共享，audio, video, screen 不可同时为 true，更不可同时为 false
      microphoneId?: ''
      // 选填，指定使用的麦克风设备的ID，可通过 getMicrophones 方法查询获得该ID，不填时，将使用默认麦克风设备
      cameraId?: ''
      // 选填，指定使用的摄像头设备的ID，可以通过 getCameras 方法查询获得该ID，不填时，将使用默认的摄像头设备
      extensionId?: ''
      // 选填，指定使用的 Chrome 插件的 extensionId，可使 72 以下版本的 Chrome 浏览器进行屏幕共享。
    }, onFailure)
}
```

### 结束连麦

- 取消发布流`unpublish`，结束本次连麦。
- 更改用户权限`role`为 `pull`。


#### 示例代码

```js
client.unpublish(StreamId, function(){
    const result = client.setRole('pull')//result: boolean 类型，成功时为 true，失败时为 false
}, function(){
    //取消本地发布失败
})// 取消发布本地流
```

### 开发注意事项

- 设置用户角色`setRole`方法，仅适用于互动直播的模式（live 模式）。加入房间前、加入房间后，都可通过调用本方法设置用户角色。

## ** Windows **

### 开始连麦

- 在使用用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB` 的情况下，开始连麦时，要更改用户权限`setStreamRole`为`UCLOUD_RTC_USER_STREAM_ROLE_BOTH`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```cpp
///发布
	///@param type 媒体类型 摄像头或者桌面
	///@param hasvideo 是否带视频
	///@param hasaudio 是否带音频
	///@return 0 succ
	virtual int publish(eUCloudRtcMeidaType type, bool hasvideo, bool hasaudio) = 0;

//媒体类型
typedef enum {
	//无
	UCLOUD_RTC_MEDIATYPE_NONE = 0,
	//摄像头流(音轨和视频轨)
	UCLOUD_RTC_MEDIATYPE_VIDEO = 1,
	//桌面
	UCLOUD_RTC_MEDIATYPE_SCREEN = 2
}eUCloudRtcMeidaType;

//关闭自动发布
sdkEngine.setAutoPublishSubscribe(false,true);
sdkEngine.configLocalScreenPublish(false);
sdkEngine.configLocalCameraPublish(false);
sdkEngine.configLocalAudioPublish(false);

//更改权限同时具备发布和订阅
sdkEngine.setStreamRole(UCloudRtcSdkStreamRole.UCLOUD_RTC_SDK_STREAM_ROLE_BOTH);

//发布摄像头流，并带支持音频和视频
sdkEngine.publish(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO, true, true);

```

在收到onLocalPublish成功后进行startPreview，开启渲染
在收到onSubscribeResult成功后进行startRemoteView 开启远端渲染

### 结束连麦


- 取消发布流`unpublish`，结束本次连麦。
- 调用`stopPreview` 停止本地渲染。
- 在结束连麦时，更改用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB`。


#### 示例代码

```cpp
    ///取消发布
	///@param type 媒体类型
	///@return 0 succ
	virtual int unPublish(eUCloudRtcMeidaType type) = 0;
    
    sdkEngine.unPublish(UCLOUD_RTC_MEDIATYPE_VIDEO);
    tUCloudRtcStreamInfo info;
    info.mUserId = "sa";
    
    sdkEngine.setStreamRole(UCloudRtcSdkStreamRole.UCLOUD_RTC_USER_STREAM_ROLE_SUB);
```

### 开发注意事项

 - 手动连麦时，需关闭自动发布功能。    


## ** Android **

### 开始连麦

- 在使用用户权限`setStreamRole`为 `UCLOUD_RTC_SDK_STREAM_ROLE_SUB` 的情况下，开始连麦时，更改用户权限`setStreamRole`为`URTC_SDK_STREAM_ROLE_BOTH`。
- 发布流`publish`，就能与老师、主播互动了。

#### 实现方法

```java
    /**
     * 发布媒体流
     * @param mtype 参见 {@link UCloudRtcSdkMediaType}
     * @param hasvideo 发布流是否带视频
     * @param hasaudio 发布流是否带音频
     * @return  错误码，参见 {@link com.ucloudrtclib.sdkengine.define.UCloudRtcSdkErrorCode}
     */
    public UCloudRtcSdkErrorCode publish(UCloudRtcSdkMediaType mtype, boolean hasvideo, boolean hasaudio);
    
    public enum UCloudRtcSdkMediaType {
    /*初始化类型，代表没有*/
    UCLOUD_RTC_SDK_MEDIA_TYPE_NULL,
    /*摄像头*/
    UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO,
    /*屏幕*/
    UCLOUD_RTC_SDK_MEDIA_TYPE_SCREEN
    }
```

#### 示例代码

```java
//关闭自动发布
sdkEngine.setAutoPublish(false);
//更改权限同时具备发布和订阅
sdkEngine.setStreamRole(UCloudRtcSdkStreamRole.UCLOUD_RTC_SDK_STREAM_ROLE_BOTH);

//发布摄像头流，并带支持音频和视频
sdkEngine.publish(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO, true, true);
```

### 结束连麦

- 取消发布流`unpublish`，结束本次连麦。
- 在结束连麦时，更改用户权限`setStreamRole`为`UCLOUD_RTC_SDK_STREAM_ROLE_SUB`。

#### 示例代码

```java

//取消发布流
sdkEngine.unpublish(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO);
//更改权限为订阅
sdkEngine.setStreamRole(UCloudRtcSdkStreamRole.UCLOUD_RTC_SDK_STREAM_ROLE_SUB);

```

### 开发注意事项

- 1.支持摄像头和屏幕同时发布。    
- 2.手动连麦时，需关闭自动发布功能。    
- 3.推送屏幕流时，只支持视频发布，即publish第三个参数必须设置为false。    

## ** iOS **

### 开始连麦

- 在使用用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload` 的情况下，开始连麦时，更改用户权限`role`为`UCloudRtcEngine_StreamProfileAll`。
- 发布流`publish`，就能与老师、主播互动了。

### 实现方法

```objc
// 流媒体类型
typedef NS_ENUM(NSInteger, UCloudRtcStreamMediaType) {
    UCloudRtcStreamMediaTypeCamera = 1,  // 摄像头
    UCloudRtcStreamMediaTypeScreen = 2, // 桌面
};

/**
 @brief 发布
 @param mediaType 流媒体类型:摄像头、桌面
*/
- (void)publishWithMediaType:(UCloudRtcStreamMediaType)mediaType;

/**
 @brief 取消发布
 @param mediaType 流媒体类型:摄像头、桌面
*/
- (void)unpublishWithMediaType:(UCloudRtcStreamMediaType)mediaType;
```

#### 示例代码

加入房间前设置:
```objc
// 设置为false，要连麦需要手动发布 “publishWithMediaType:”
self.sdkEngine.isAutoPublish = false;

// 可以关闭音频
self.sdkEngine.enableLocalAudio = NO;
// 可以关闭视频
self.sdkEngine.enableLocalVideo = NO;
```

加入房间之后，互动连麦，发布本地流：

```objc
// 连麦前修改权限
self.sdkEngine.streamProfile = UCloudRtcEngine_StreamProfileAll;
// 连麦
[self.sdkEngine publishWithMediaType:UCloudRtcStreamMediaTypeCamera];
```

```swift
self.sdkEngine?.streamProfile = .streamProfileAll
self.sdkEngine?.publish(with: .camera)
```

### 结束连麦

- 取消发布流`unPublish`，就结束本次连麦。
- 在结束连麦时，更改用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload`。

#### 示例代码

```objectivec
    // 结束本次连麦
    [self.sdkEngine unpublishWithMediaType:UCloudRtcStreamMediaTypeCamera];
    
    // 只有拉流权限
    self.sdkEngine.streamProfile = UCloudRtcEngine_StreamProfileDownload;
```

```swift
self.sdkEngine?.unpublish(with: .camera)
self.sdkEngine?.streamProfile = .streamProfileDownload
```
### 开发注意事项

- 1.需要手动控制连麦，将`isAutoPublish`设置为`false`；    
- 2.加入频道前，可以通过设置`enableLocalAudio`、`enableLocalVideo`关闭 音频和视频，默认音频和视频都是开启的；



<!-- tabs:end -->
