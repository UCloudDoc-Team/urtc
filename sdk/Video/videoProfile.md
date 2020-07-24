# 设置视频参数

可以调整视频画面的清晰度和流畅度，获得较高的用户体验。    

通过视频的profile方法来设置包含视频分辨率、帧率、码率、方向模式等参数。   


<!-- tabs:start -->

## ** Web **
 

### 实现方法

设置视频的 profile（通过getSupportProfileNames获取到视频质量的值，不设置时，默认为 "640*480"）限制 client 使用的视频大小、帧率、带宽等。 

### 示例代码
```js
client.setVideoProfile({
  previewId?: string,   // 选填，本地（预览）流的 previewId，不填时，为修改全局的 video profile 设置，供后续创建或发布的流使用
  profile: string      // 必填，指定 video profile
}, function() {
 //成功时回调
}, function(Err){
 //失败时回调，Err 为错误信息
})
```  


## ** Windows **

### 实现方法

balabala……    

### 示例代码

balabala……  

## ** Android **

可以调整视频画面的清晰度和流畅度，获得较高的用户体验。    

通过setVideoProfile方法来设置包含视频分辨率、帧率、码率等参数。  

### 实现方法

public UCloudRtcSdkErrorCode setVideoProfile(UCloudRtcSdkVideoProfile profile);

UCloudRtcSdkVideoProfile类型说明如下：
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
     * 用于外部扩展接入时，会把最低码率，起始码率设置成1500 kbps 可调用extendParams 设置fps,width * height，不设置默认值为25fps,640 * 480
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
//设置640*480分辨率
sdkEngine.setVideoProfile(UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480);

``` 

## ** iOS **
  

### 实现方法

balabala……    

### 示例代码

balabala……  

## ** macOS **
    

### 实现方法

balabala……    

### 示例代码

balabala……  

<!-- tabs:end -->

# 常用分辨率、帧率、码率

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
"1920\*1080" | 1920\*1080 | 15 | 1500
"1920\*1080_1" | 1920\*1080 | 15 | 2000
"1920\*1080_2" | 1920\*1080 | 30 | 2500

