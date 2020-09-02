# 互动连麦

在大班课、互动直播的过程中，学生、观众可以连麦，与老师、主播进行互动，丰富课堂、直播体验，提升教学、直播效果。

<!-- tabs:start -->

## ** Web **

### 开始连麦

- 在使用用户权限`role`为 `pull` 的情况下，开始连麦时，要更改用户权限`role`为`push-and-pull`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```js
待更新
```

### 结束连麦

- 更改用户权限`role`为 `pull`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```js
待更新
```

### 开发注意事项

待更新

## ** Windows **

### 开始连麦

- 在使用用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB` 的情况下，开始连麦时，要更改用户权限`setStreamRole`为`UCLOUD_RTC_USER_STREAM_ROLE_BOTH`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```cpp
待更新
```

### 结束连麦

- 在结束连麦时，更改用户权限`setStreamRole`为 `UCLOUD_RTC_USER_STREAM_ROLE_SUB`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```cpp
待更新
```

### 开发注意事项

待更新

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

- 在结束连麦时，更改用户权限`setStreamRole`为`UCLOUD_RTC_SDK_STREAM_ROLE_SUB`。
- 取消发布流`unpublish`，结束本次连麦。

#### 示例代码

```java
//更改权限为订阅
sdkEngine.setStreamRole(UCloudRtcSdkStreamRole.UCLOUD_RTC_SDK_STREAM_ROLE_SUB);
//结束摄像头发布
sdkEngine.unpublish(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO);
```

### 开发注意事项

1.支持摄像头和屏幕同时发布。    
2.手动连麦时，需关闭自动发布功能。    
3.推送屏幕流时，只支持视频发布，即publish第三个参数必须设置为false。    

## ** iOS **

### 开始连麦

- 在使用用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload` 的情况下，开始连麦时，更改用户权限`role`为`UCloudRtcEngine_StreamProfileAll`。
- 发布流`publish`，就能与老师、主播互动了。

#### 示例代码

```objectivec
待更新
```

```swift
待更新
```

### 结束连麦

- 在结束连麦时，更改用户权限`UCloudRtcEngineStreamProfile`为 `UCloudRtcEngine_StreamProfileDownload`。
- 取消发布流`unPublish`，就结束本次连麦。

#### 示例代码

```objectivec
待更新
```

```swift
待更新
```
### 开发注意事项

待更新



<!-- tabs:end -->
