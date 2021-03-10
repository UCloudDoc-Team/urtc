# 设置视频参数

可以调整视频画面的清晰度和流畅度，获得较高的用户体验。    
通过视频的profile方法来设置包含视频分辨率、帧率、码率等参数。   


<!-- tabs:start -->

## ** Web **
 

### 实现方法

设置视频的 `profile` 限制 客户端 使用的视频大小、帧率、带宽等。 

> 设置之前，可以先通过`getSupportProfileNames`获取到视频质量支持的值，不设置时，默认为 "640*480"。

### 示例代码

```js
client.setVideoProfile({
  previewId?: string,   // 选填，本地（预览）流的 previewId，不填时，为修改全局的 video profile 设置，供后续创建或发布的流使用
  profile: "1280*720"      // 必填，指定 video profile为 720P
}, function() {
 //成功时回调
}, function(Err){
 //失败时回调，Err 为错误信息
})
```  


## ** Windows **

### 实现方法

```cpp
//设置编码发送视频质量
//@param profile 分辨率
//@param videoconfig video配置
virtual void setVideoProfile(eUCloudRtcVideoProfile profile, tUCloudVideoConfig& videoconfig) = 0;
 
//设置采集渲染视频分辨率
//@param profile 分辨率
virtual void setVideoCaptureProfile(eUCloudRtcVideoProfile profile) = 0;
	
//设置桌面分享采集发送profile
//@param profile 分辨率
virtual void setDesktopProfile(eUCloudRtcScreenProfile profile) = 0;
```

### 示例代码

```cpp
//创建引擎，加入房间成功后
auto engine = UCloudRtcEngine::sharedInstance();
....
//设置cam采集和发送的分辨率
//视频profile
typedef enum {
	UCLOUD_RTC_VIDEO_PROFILE_NONE = -1, 
	UCLOUD_RTC_VIDEO_PROFILE_320_180 = 1,
	UCLOUD_RTC_VIDEO_PROFILE_320_240 = 2,
	UCLOUD_RTC_VIDEO_PROFILE_640_360 = 3,
	UCLOUD_RTC_VIDEO_PROFILE_640_480 = 4,
	UCLOUD_RTC_VIDEO_PROFILE_1280_720 = 5,
	UCLOUD_RTC_VIDEO_PROFILE_1920_1080 = 6,
	UCLOUD_RTC_VIDEO_PROFILE_240_180 = 7,
	UCLOUD_RTC_VIDEO_PROFILE_480_360 = 8,
	UCLOUD_RTC_VIDEO_PROFILE_960_720 = 9
} eUCloudRtcVideoProfile;

tUCloudVideoConfig objConfig;
engine->setVideoProfile(UCLOUD_RTC_VIDEO_PROFILE_1280_720,objConfig); 
//当指定UCLOUD_RTC_VIDEO_PROFILE_NONE，以第二个参数传入实际值为准
engine->setVideoCaptureProfile(UCLOUD_RTC_VIDEO_PROFILE_1280_720);

//设置桌面的采集分辨率
//桌面profile
typedef enum {
	UCLOUD_RTC_SCREEN_PROFILE_LOW = 1,    //分辨率180P
	UCLOUD_RTC_SCREEN_PROFILE_MIDDLE = 2,    //分辨率360P
	UCLOUD_RTC_SCREEN_PROFILE_HIGH = 3,    //分辨率720P 1M码率 5帧
	UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS = 4,    //分辨率1080P 1.5M码率 5帧
	UCLOUD_RTC_SCREEN_PROFILE_HIGH_1 = 5,     //分辨率720P 2M码率 25帧
	UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS_1 = 6, //分辨率1080P 最大2.5M码率 25帧
	UCLOUD_RTC_SCREEN_PROFILE_HIGH_0 = 7,     //分辨率720P 1.6M码率 15帧
	UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS_0 = 8 //分辨率1080P 2M码率 18帧
} eUCloudRtcScreenProfile;
engine->setDesktopProfile(UCLOUD_RTC_SCREEN_PROFILE_MIDDLE);

//离开房间销毁引擎
engine->leaveChannel();
engine->destroy();

```

## ** Android **

### 实现方法

```java
public UCloudRtcSdkErrorCode setVideoProfile(UCloudRtcSdkVideoProfile profile);
```

`UCloudRtcSdkVideoProfile`类型说明如下：

```java
    /**
     * 视频配置分辨率 320*180,对应传输最小码率 100kbps 最大码率 200kbps 初始码率 100kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_320_180,
    /**
     * 视频配置分辨率 352_288,对应传输最小码率 100kbps 最大码率 300kbps 初始码率 200kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_352_288,
    /**
     * 视频配置分辨率 480_360,对应传输最小码率 100kbps 最大码率 400kbps 初始码率 200kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_480_360,
    /**
     * 视频配置分辨率 640_360,对应传输最小码率 200kbps 最大码率 400kbps 初始码率 300kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_640_360,
    /**
     * 视频配置分辨率 640_480,对应传输最小码率 200kbps 最大码率 500kbps 初始码率 300kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480,
    /**
     * 视频配置分辨率 1280_720,对应传输最小码率 500kbps 最大码率 1000kbps 初始码率 600kbps
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_1280_720,

    /**
     * 用于外部扩展接入时，会把最低码率，起始码率设置成1500 kbps 可调用extendParams 设置fps，width * height，不设置默认值为25fps，640 * 480
     * 同时会影响到录像的分辨率，相关请参考{@link UCloudRtcSdkCaptureMode}
     */
    UCLOUD_RTC_SDK_VIDEO_PROFILE_EXTEND,

    /**
     * 标清,关联录制房间视频的分辨率 640*360以下
     */
    UCLOUD_RTC_SDK_VIDEO_RESOLUTION_STANDARD,

    /**
     * 高清，关联录制房间视频的分辨率 720p及以下
     */
    UCLOUD_RTC_SDK_VIDEO_RESOLUTION_HIGH,

    /**
     * 超清，关联录制房间视频的分辨率 720p及以上
     */
    UCLOUD_RTC_SDK_VIDEO_RESOLUTION_SUPER_HIGH;

``` 

### 示例代码

```java
//设置720P分辨率
sdkEngine.setVideoProfile(UCLOUD_RTC_SDK_VIDEO_PROFILE_1280_720);

``` 

## ** iOS **
  
### 实现方法

请在加入房间之前设置，暂不支持加入房间后动态修改分辨率，枚举值如下:

```objectivec
typedef NS_ENUM(NSInteger)
{
    UCloudRtcEngine_VideoProfile_180P = 0,   // 分辨率:240*180,  码率范围:100-200kpbs, 帧率:15fps
    UCloudRtcEngine_VideoProfile_360P_1 = 1, // 分辨率:480*360,  码率范围:100-300kpbs, 帧率:15fps(默认值)
    UCloudRtcEngine_VideoProfile_360P_2 = 2, // 分辨率:640*360,  码率范围:100-400kpbs, 帧率:20fps
    UCloudRtcEngine_VideoProfile_480P = 3,   // 分辨率:640*480,  码率范围:100-500kpbs, 帧率:20fps
    UCloudRtcEngine_VideoProfile_720P = 4,   // 分辨率:1280*720, 码率范围:300-1000kpbs,帧率:30fps
    UCloudRtcEngine_VideoProfile_1080P = 5,  // 分辨率:1920*1080,码率范围:500-1500kpbs,帧率:30fps
} UCloudRtcEngineVideoProfile;
```


### 示例代码

``` objectivec
// 设置分辨率720P
sdkEngine.videoProfile = UCloudRtcEngine_VideoProfile_720P;
```  

<!-- tabs:end -->

## 开发注意事项

设置视频参数，需要在**发布之前**使用。


## 常用分辨率、帧率、码率

视频分辨率，一般根据场景不同、客户喜好不同，有多种设置选项。常用分辨率见下表，可以根据需要，设置对应的分辨率。    
除了常用分辨率，也可以使用自定义的参数接口，自行设置分辨率、帧率、码率。    

名称 | 视频宽高 | 帧率(fps) | 视频码率(Kbps)
:-: | :-: | :-: | :-:
"240\*180" | 240\*180 | 20 | 200
"320\*180" | 320\*180 | 20 | 300
"320\*240" | 320\*240 | 20 | 400
"480\*360" | 480\*360 | 20 | 400
"640\*360" | 640\*360 | 20 | 500
"640\*480" | 640\*480 | 20  | 600
"1280\*720" | 1280\*720 | 15 | 1000
"1280\*720_1" | 1280\*720 | 15 | 1500
"1280\*720_2" | 1280\*720 | 30 | 2000
"1280\*720_3" | 1280\*720 | 5 | 1000
"1920\*1080" | 1920\*1080 | 15 | 1500
"1920\*1080_1" | 1920\*1080 | 15 | 2000
"1920\*1080_2" | 1920\*1080 | 30 | 2500
"1920\*1080_3" | 1920\*1080 | 5 | 1500


