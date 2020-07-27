# 视频采集旋转


<!-- tabs:start -->

# ** iOS **

## 实现方法

通过设置采集旋转方向，保证视频和StatusBar的相对位置在采集端和播放端始终一致，视频采集旋转方向与 `UIInterfaceOrientation`方向一致。

```objc
typedef NS_ENUM(NSUInteger, UCloudRtcOrientationMode) {
    UCloudRtcOrientationModeAdaptive,       // 自适应模式
    UCloudRtcOrientationModeLandscapeLeft,  // Home键位置向左
    UCloudRtcOrientationModeLandscapeRight, // Home键位置向右
};
```

- `UCloudRtcOrientationModeAdaptive`模式：采集方向会根据重力感应自动调整，使采集端和播放端始终一致。
- `UCloudRtcOrientationModeLandscapeLeft`模式：设备横屏，固定Home键向左，该模式下，输出的视频相对`StatusBar`总是处于横屏模式。
- `UCloudRtcOrientationModeLandscapeRight`模式：设备横屏，固定Home键向右，该模式下，输出的视频相对`StatusBar`总是处于横屏模式。

示例代码：

```objc
// 固定横屏 Home键位置向左
- (UIInterfaceOrientationMask)supportedInterfaceOrientations
{
    return UIInterfaceOrientationMaskLandscapeLeft;
}
```

``` objc
// 根据屏幕旋转方向设置视频采集方向
sdkEngine.orientationMode = UCloudRtcOrientationModeLandscapeLeft;
```


## ** Android **

### 实现方法

文档待更新

<!-- tabs:end -->
