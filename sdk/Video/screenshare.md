# 屏幕共享

屏幕共享，可以把客户端的屏幕，以视频的方式分享给其他人。支持分享整个屏幕、分享应用、分享浏览器窗口。


<!-- tabs:start -->

## ** Web **

在开始屏幕共享前，请确保集成Web端SDK，详见[快速集成SDK](https://github.com/ucloud/urtc-sdk-web/blob/master/Manual.md) 。

### 无插件屏幕共享

下表中的浏览器，可以使用无插件屏幕分享。   

| windows 系统 | macOS 系统 |
|-|-|
| Chrome 72+ | Chrome 72+ |
| Firefox 66+ | Firefox 66+|
| Opera 60+ | Opera 60+ |
| Edge 79+ | Edge 79+ |
| 360 极速浏览器12+ | 360 极速浏览器12+  |
| 360 安全浏览器 12+ | NA |
| NA | 苹果Safari 13+ |


在以上浏览器中共享屏幕时 调用`publish`方法把`video`字段设为 `false`， `screen` 字段设为 `true` 即可。   

```js
client.publish(
  {
    audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

### 使用屏幕共享插件

chrome 72及72版本以上无需下载插件，72版本以下则需要下载[URTC屏幕共享插件](http://urtcsdk.cn-bj.ufileos.com/URTC-screen-extention.zip)。

打开你的 Chrome 浏览器，点击屏幕右上方的扩展按钮，选择 更多工具 > 扩展程序， 打开开发者模式 > 加载已解压的扩展程序 > 选择 解压的 URTC屏幕共享插件文件夹，即可完成安装

安装 URTC 提供的 Chrome 屏幕共享插件 ，并获取插件的 `extensionId`，在 publish 的时候填入 `extensionId`。

```js
client.publish(
  {
    audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
    extensionId?: string // 选填，指定使用的Chrome插件的 extensionId，使用Chrome屏幕共享插件时必填
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

* audio 属性建议设置为 false，避免订阅端收到的两路流中都有音频，导致回声，建议音频只推一路流，防止出现回音。
* 在本地共享的时候，本地流的 Client 不要订阅本地的媒体流，否则会增加时长计费。
* 创建屏幕共享流的时候，video 必须设置为 false。如果屏幕分享需要混音共享流，参考 [播放混音](urtc/sdk/Audio/AudioMixing)的方法。

## ** Windows **

### 实现方法

Windows SDK支持分享桌面、窗口、指定区域。

#### 共享指定屏幕

发布本地桌面指定屏幕可以按照以下步骤进行操作（代码为伪代码，实际运行请参照伪代码步骤实现）：

示例代码：

```cpp
setUseDesktopCapture(UCLOUD_RTC_DESKTOPTYPE_SCREEN);
setDesktopProfile(UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS);

int num = getDesktopNums();
tUCloudRtcDeskTopInfo[num];
for(int i = 0;i < num;i++){
  getDesktopInfo(i,tUCloudRtcDeskTopInfo[i]);
}
tUCloudRtcScreenPargram screenSet;
screenSet.mScreenindex = tUCloudRtcDeskTopInfo[0].mDesktopId;
screenSet.mXpos = 0;
screenSet.mYpos = 0;
screenSet.mWidth = tUCloudRtcDeskTopInfo[0].mScreenW;
screenSet.mHeight =  tUCloudRtcDeskTopInfo[0].mScreenH;
setCaptureScreenPagrams(screenSet);

publish(UCLOUD_RTC_MEDIATYPE_SCREEN,true,true);
```

#### 共享指定窗口

发布本地桌面指定窗口可以按照以下步骤进行操作（代码为伪代码，实际运行请参照伪代码步骤实现）

示例代码：

```cpp
setUseDesktopCapture(UCLOUD_RTC_DESKTOPTYPE_WINDOW);
setDesktopProfile(UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS);

std::string strWindowTitle = "your window";
int num = getWindowNums();
tUCloudRtcDeskTopInfo[num];
for(int i = 0;i < num;i++){
  getWindowInfo(i,tUCloudRtcDeskTopInfo[i]);
  if( strcmp(tUCloudRtcDeskTopInfo[i].mDesktopTitle,strWindowTitle.data()) == 0 ){
    tUCloudRtcScreenPargram windowSet;
      windowSet.mScreenindex = tUCloudRtcDeskTopInfo[0].mDesktopId;
      windowSet.mXpos = 0;    //x坐标
      windowSet.mYpos = 0;    //y坐标
      windowSet.mWidth = w;   //分享窗口得宽度
      windowSet.mHeight =  h; //分享窗口得高度
      setCaptureScreenPagrams(windowSet);
      publish(UCLOUD_RTC_DESKTOPTYPE_WINDOW,true,true);
      break;
  }
}


```

### 开发注意事项

## ** Android **

### 实现方法

Android SDK支持分享设备的整个屏幕，屏幕流可以根据需要自动发布或者手动发布。

#### 自动发布屏幕流
加入房间后，屏幕流会自动推送。

```java
//设定支持屏幕推流
sdkEngine.configLocalScreenPublish(true);
//设置为自动发布
sdkEngine.setAutoPublish(true);
```

#### 手动发布屏幕流
加入房间后，屏幕流根据需要调用发布接口。

```java
//设定支持屏幕推流
sdkEngine.configLocalScreenPublish(true);
//设置为手动发布
sdkEngine.setAutoPublish(false);
```

```java
//根据需要发布屏幕流
sdkEngine.publish(UCLOUD_RTC_SDK_MEDIA_TYPE_SCREEN, true, false);
```

### 开发注意事项


## ** iOS **

### 实现方法

iOS SDK支持分享整个屏幕。

#### 发布屏幕流
加入房间后，屏幕流根据需要调用发布接口。

```objectivec
[sdkEngine publishWithMediaType:UCloudRtcStreamMediaTypeScreen];
```
#### 取消发布屏幕流

```objectivec
[sdkEngine unpublishWithMediaType:UCloudRtcStreamMediaTypeScreen];
```

### 开发注意事项






<!-- tabs:end -->
