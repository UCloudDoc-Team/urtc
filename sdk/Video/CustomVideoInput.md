# 自定义视频采集
<!-- {docsify-ignore-all} -->
<!-- tabs:start -->

## ** Android **

实时音视频应用中，通常会使用默认的音视频采集模块，在部分场景中，需要自定义视频采集。
本文介绍如何实现通过外部视频源，实现自定义视频采集。

### 实现方法

Android sdk支持`i420`、`nv21`、`nv12`等`yuv420p`系列的外部源以及`rgba`、`abgr`、`rgb565`等`rgba`系列数据，作为自定义视频源。    

### 示例代码

```java
public interface UcloudRTCDataProvider {

    //0-3 表示转换类型
    //4-7 表示rgba_stride的宽度的倍数
    //8-11 表示yuv_stride宽度移位数
    //12-15 表示uv左移位数
    public static final int RGBA_TO_I420 = 0x01001040;
    public static final int ABGR_TO_I420 = 0x01001041;
    public static final int BGRA_TO_I420 = 0x01001042;
    public static final int ARGB_TO_I420 = 0x01001043;
    public static final int RGB24_TO_I420 = 0x01001034;
    public static final int RGB565_TO_I420 = 0x01001025;
    public static final int NV21 = 0x01001090;
    public static final int NV12 = 0x01001091;
    public static final int I420 = 0x01001099;

    /**
     * 供sdk采集外部数据进行推流的方法，由sdk 使用者来提供数据，具体使用方式请参考rgb转yuv使用说明
     * @param params  需要传入3个参数 ，参数顺序不可颠倒，参数1：类型，例如RGBA_TO_I420，参数2：宽 参数3： 高
     * @return  扩展推流的原始rgb数据或者yuv420p，NV21
     */
    ByteBuffer provideRGBData(List<Integer> params);


    /**
     * 释放申请的buffer
     */
    void releaseBuffer();
}

```
设置外部采集模式参数

```java
 //设置sdk 外部扩展模式及其采集的帧率，同时sdk内部会自动调整初始码率和最小码率
 //扩展模式只支持720p的分辨率及以下，若要自定义更高分辨率，请联系Ucloud商务定制，否则sdk会抛出异常，终止运行。
 sdkEngine.setVideoProfile(UCloudRtcSdkVideoProfile.UCLOUD_RTC_SDK_VIDEO_PROFILE_EXTEND.extendParams(30,640,480));
//设置捕获模式，二选一
//        //普通摄像头捕获方式，与扩展模式二选一
//        UCloudRtcSdkEnv.setCaptureMode(
//                UCloudRtcSdkCaptureMode.UCLOUD_RTC_CAPTURE_MODE_LOCAL);
        //扩展摄像头或外部数据数据捕获，与普通捕获模式二选一
        UCloudRtcSdkEnv.setCaptureMode(
                UCloudRtcSdkCaptureMode.UCLOUD_RTC_CAPTURE_MODE_EXTEND);
```
### 开发注意事项

调用范例，具体内容请参考demo源码内`UCloudRTCLiveActivity`，需要根据自己的实际情况来，范例只是做个参考。

```java

    // 创建缓存空间
    private ByteBuffer videoSourceData = sdkEngine.getNativeOpInterface().
                    createNativeByteBuffer(1280 * 720 * 4);
 
    // 外部ByteBuffer视频数据保存到缓存中
    private void createFrameByteBuffer(ByteBuffer frame) { // 获取外接视频数据
        try {
            if (frame != null) {
                synchronized (extendByteBufferSync) {
                    if (videoSourceData != null) {
                        videoSourceData.clear();
                        videoSourceData.put(frame);
                        videoSourceData.flip();
                    }
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

```

```java
    //sdk作为消费者消费数据
    private UCloudRTCDataProvider mUCloudRTCDataProvider = new UCloudRTCDataProvider() {
        private ByteBuffer cacheBuffer;

        @Override
        public ByteBuffer provideRGBData(List<Integer> params) {
            if (videoSourceData == null ) {
                Log.d("UCloudRTCLiveActivity", "provideRGBData byteBuffer data is null");
                return null;
            } else {
                params.add(UCloudRTCDataProvider.I420);
                params.add(640);
                params.add(480);
                if (cacheBuffer == null) {
                    cacheBuffer = sdkEngine.getNativeOpInterface().
                            createNativeByteBuffer(1280 * 720 * 4);
                    Log.d("UCloudRTCLiveActivity", "byteBuffer createNativeByteBuffer call ");
                    cacheBuffer.clear();
                } else {
                    cacheBuffer.rewind();
                }
                synchronized (extendByteBufferSync) {
                    cacheBuffer.put(videoSourceData);
                    videoSourceData.rewind();
                }

                cacheBuffer.flip();
                return cacheBuffer;
            }
        }

        @Override
        public void releaseBuffer() { // 释放资源
            Log.d("UCloudRTCLiveActivity", "releaseBuffer");
            synchronized (extendByteBufferSync) {
                if (videoSourceData != null) {
                    videoSourceData.clear();
                    sdkEngine.getNativeOpInterface().realeaseNativeByteBuffer(videoSourceData);
                    videoSourceData = null;
                }
            }
            if (cacheBuffer != null) {
                cacheBuffer.clear();
                sdkEngine.getNativeOpInterface().realeaseNativeByteBuffer(cacheBuffer);
                cacheBuffer = null;
            }
        }
    };
```
## ** iOS **

默认内置的摄像头作为视频输入设备，将采集的视频推流。 如果需要自定义视频源进行推流，请将enableExtendVideoCapture设置为YES， 再将自定义的视频源发送给 UCloudRtcEngine。


开启自定义视频源功能
```objc
self.manager.enableExtendVideoCapture = YES;
```

设置自定义视频源分辨率、帧率
```objc
UCloudRtcVideoFrame *videoFrame = [[UCloudRtcVideoFrame alloc] initVideoFrameWithWidth:320 height:180 fps:20];
self.manager.extendVideoFrame = videoFrame;
```

将每帧视频源发送给 UCloudRtcEngine
```objc
/**
 *@brief 上传自定义视频
 *@param pixelBuffer 每帧图片信息
 *@param timestamp 每帧对应的时间
 *@param rotation 旋转方向
*/
- (void)publishPixelBuffer:(CVPixelBufferRef _Nonnull)pixelBuffer timestamp:(CMTime)timestamp rotation:(UCloudRtcVideoRotation)rotation;
```

### 开发注意事项

1.默认使用SDK的摄像头采集视频，即默认enableExtendVideoCapture=NO，若设置为YES，开启自定义视频源数据，并请在加入房间后，发流前设置； 
2.外部采集有两种模式， 拉模式和推模式。  
    2.1 拉模式使用摄像头采集线程回调UCloudIVideoFrameObserver接口获取数据，此模式会占用摄像头
        设置顺序如下：
        enableExtendVideocapture();
        SetExtendMediaDataMode(UCloud_EMDM_PULL);
        registerVideoFrameObserver();
        
    2.2 推模式外部直接调用接口pushVideoFrameData pushAudioFrameData 推送数据给SDK， 推送的数据里需要包含时间戳信息。
        设置顺序如下：
        enableExtendVideocapture();
        enableExtendAudiocapture();
        SetExtendMediaDataMode(UCloud_EMDM_PUSH);
        发流成功送调用
        pushVideoFrameData();
        pushAudioFrameData();
    2.3 模式的选择在发布UCLOUD_RTC_MEDIATYPE_VIDEO前设置，发布后不能切换。
   
3.UCloudRtcEngine提供 UCloudRtcVideoFrame 对象，可扩展自定义视频分辨率和帧率，若不设置，默认为原视频源数据；



## ** Windows **

```cpp

///推送一帧外部采集的视频数据给SDK
///@param video 视频数据
///@return 0 succ
virtual int pushVideoFrameData(tUCloudRtcVideoFrame *video) = 0;

///推送一帧外部采集的音频数据给SDK
///@param audio 视频数据
///@return 0 succ
virtual int pushAudioFrameData(tUCloudRtcAudioFrame *audio) = 0;

///设置外部采集数据的获取方式 
///@param mode  UCloud_EMDM_PUSH：推送数据给sdk   UCloud_EMDM_PULL：SDK拉数据
///@return 0 succ
virtual int SetExtendMediaDataMode(eUCloudExtendMediaDataMode mode) = 0;
    
//该方法用于注册视频观测器对象
//@param codec 编码类型
virtual void registerVideoFrameObserver(UCloudIVideoFrameObserver *observer) = 0;

//开启外部采集视频
//@param enable 是否使用扩展的外部采集摄像头
//@param videocapture 外部视频源
//@return 0 succ
virtual int enableExtendVideocapture(bool enable, UCloudRtcExtendVideoCaptureSource* videocapture) = 0;
    
//视频监听回调
class _EXPORT_API UCloudIVideoFrameObserver
    {
    public:
        //视频采集到每一帧得回调
        //@param videoframe 视频数据
        virtual  bool onCaptureFrame(tUCloudRtcVideoFrame *videoFrame) = 0;

    };
    
```
    
### 开发注意事项
   
初始化引擎后：    
1.调用`registerVideoFrameObserver` 注册相关的视频数据回调监听实例。    
2.当需要使用外置视频源时 调用`enableExtendVideocapture(true,nullptr)`。    
3.在回调类中实现`onCaptureFrame`接口，将自定义采集数据通过回调接口之间进行替换，拷贝到`videoFrame`的`mDataBuf`，并且返回true，即可完成外置源的数据送入。       
4.当需要切换回内置数据采集时`enableExtendVideocapture(false,customCapture)`。    
   
> 注意：windows支持最大1920*1080p的yuvi420的数据，进行替换时需要先进行转成yuvi420数据进行替换。

<!-- tabs:end -->
