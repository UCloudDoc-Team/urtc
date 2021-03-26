# 视频采集旋转


<!-- tabs:start -->

## ** iOS **

### 实现方法

通过设置采集旋转方向，保证视频和StatusBar的相对位置在采集端和播放端始终一致，视频采集旋转方向与 `UIInterfaceOrientation`方向一致。

```objectivec
typedef NS_ENUM(NSUInteger, UCloudRtcOrientationMode) {
    UCloudRtcOrientationModeAdaptive,       // 自适应模式
    UCloudRtcOrientationModeLandscapeLeft,  // Home键位置向左
    UCloudRtcOrientationModeLandscapeRight, // Home键位置向右
};
```

- `UCloudRtcOrientationModeAdaptive`模式：采集方向会根据重力感应自动调整，使采集端和播放端始终一致。
- `UCloudRtcOrientationModeLandscapeLeft`模式：设备横屏，固定Home键向左，输出的视频相对`StatusBar`总是处于横屏模式。
- `UCloudRtcOrientationModeLandscapeRight`模式：设备横屏，固定Home键向右，输出的视频相对`StatusBar`总是处于横屏模式。

### 示例代码

```objectivec
// 固定横屏 Home键位置向左
- (UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskLandscapeLeft;
}
```

``` objectivec
// 根据屏幕旋转方向设置视频采集方向
sdkEngine.orientationMode = UCloudRtcOrientationModeLandscapeLeft;
```


## ** Android **



### 实现方法

通过设置视频采集旋转方向，保证视频在采集端和播放端始终一致，设置模式一共有三种，横屏竖屏和自动旋转。

```java
public enum UCloudRtcSdkPushOrentation {
    /*自动模式，会根据设备方向和摄像头方向进行计算*/
    UCLOUD_RTC_PUSH_AUTO_MODE,
    /*固定横屏模式，固定设备方向为90°*/
    UCLOUD_RTC_PUSH_LANDSCAPE_MODE,
    /*固定竖屏方式，固定了设备方向为0°*/
    UCLOUD_RTC_PUSH_PORTRAIT_MODE ,
}
```

### 示例代码

```java
//自动模式
UCloudRtcSdkEnv.setPushOrientation(UCloudRtcSdkPushOrentation.UCLOUD_RTC_PUSH_AUTO_MODE);

//固定横屏模式
UCloudRtcSdkEnv.setPushOrientation(UCloudRtcSdkPushOrentation.UCLOUD_RTC_PUSH_LANDSCAPE_MODE);

//固定竖屏方式
UCloudRtcSdkEnv.setPushOrientation(UCloudRtcSdkPushOrentation.UCLOUD_RTC_PUSH_PORTRAIT_MODE);
```

### 视频输出模式

除了以上提供到采集端方向设置之外，sdk还提供了视频端输出方向模式，为防止视频因旋转出现大头、缩放或剪切的问题。

setVideoOutputOrientation提供了UCloudRtcSdkVideoOutputOrientationMode参数，可以通过这个参数，结合视频场景需要，获取想要的视频渲染效果。

UCloudRtcSdkVideoOutputOrientationMode参数一共有三种方向模式：

1.Adaptive 模式

该模式下 SDK 输出的视频方向与采集到的视频方向一致。接收端会根据收到的视频旋转信息对视频进行旋转。该模式适用于接收端可以调整视频方向的场景:

如果采集的视频是横屏模式，则输出的视频也是横屏模式。
如果采集的视频是竖屏模式，则输出的视频也是竖屏模式。

2.FixedLandscape 模式

该模式下，SDK 保证输出的视频相对 Status Bar 总是处于横屏模式；如果采集到的视频是竖屏模式，则相对于播放端 Status bar 平行方向的画面会被裁剪。

3.FixedPortrait 模式

该模式下，SDK 保证输出的视频相对 Status Bar 总是处于竖屏模式；如果采集到的视频是横屏模式，则相对于播放端 Status bar 垂直方向的画面会被裁剪。

### 视频输出模式设置示例代码
```java

//视频输出方向与采集方向一致
UCloudRtcSdkEnv.setVideoOutputOrientation(UCloudRtcSdkVideoOutputOrientationMode.UCLOUD_RTC_VIDEO_OUTPUT_ADAPTIVE_MODE);

//固定横向视频输出模式
UCloudRtcSdkEnv.setVideoOutputOrientation(UCloudRtcSdkVideoOutputOrientationMode.UCLOUD_RTC_VIDEO_OUTPUT_FIXED_LANDSCAPE_MODE);

//固定垂直视频输出模式
UCloudRtcSdkEnv.setVideoOutputOrientation(UCloudRtcSdkVideoOutputOrientationMode.UCLOUD_RTC_VIDEO_OUTPUT_FIXED_PORTRAIT_MODE);
```

<!-- tabs:end -->
