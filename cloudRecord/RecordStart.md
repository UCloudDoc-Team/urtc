# 云端录制SDK代码示例

无需集成额外的SDK，通过以下方法，可以快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。

## 1. 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [云端录制](urtc/cloudRecord/index)开通录制服务。

## 2. 录制音视频代码示例

<!-- tabs:start -->

## ** Web **

## 2.1 Web开始录制

```js
client.startRecord({
  bucket: string  // 必传，存储的 bucket, URTC 使用 UCloud 的 UFile 产品进行在存储，相关信息见控制台操作文档
  region: string  // 必传，存储服务所在的地域
}, function onSuccess(Record) {

	//开始录制成功返回信息：录制的文件的名称FileName和录制编号RecordId
	//播放录制地址规则：const url = `http://${bucket}.${region}.ufileos.com/${Record.FileName}.mp4`;
}, function(Err) {

	//开始录制错误返回值

})
```

> - 不设定其他的参数时，默认会录制房间内所有的视频流，混流合成分辨率为 1280 x 720 的MP4文件。
> - 可以设定录制文件的分辨率；设置单流录制、混流录制以及混流的风格；设置录制加水印；仅录制分享的屏幕/摄像头。
> - 可以查看[startRecord 参数](https://github.com/ucloud/urtc-sdk-web#client-startrecord)，了解更多的录制参数。

## 2.2 Web停止录制

```js
client.stopRecord(function onSuccess() {

	//停止录制成功时执行的回调函数

}, function(Err) {

	//停止录制错误返回值
	
})
```  



## ** Windows **


## Windows录制音视频

```cpp
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型类型：摄像头、屏幕
recordconfig.mMainviewuid = m_userid.data(); // 主画面
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD; // 录制等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;
recordconfig.mBucket = "your bucket";
recordconfig.mBucketRegion = "your bucket region";
recordconfig.mWaterMarkType = UCLOUD_RTC_WATERMARK_TYPE_TIME;  // 水印类型
recordconfig.mWatermarkUrl = "hello urtc"; // 如果是文字水印为水印内容   如果是图片则为图片url 地址
recordconfig.mStreams = nullptr;  //指定混流的用户
recordconfig.mStreamslength = 0;  //混流的用户数
recordconfig.mLayout = 2;         //录制混流风格（1.平铺风格 2.垂直风格 3.自定义布局 4.模板自适应一 5.模板自适应二）
m_rtcengine->startRecord(recordconfig);

消息回调
//开启录制回调
virtual void onStartRecord (const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 




## ** Android **


## Android录制音视频

```java
//                如果主窗口是当前用户
UcloudRtcSdkRecordProfile recordProfile = UcloudRtcSdkRecordProfile.getInstance().assembleRecordBuilder()
                        .recordType(UcloudRtcSdkRecordProfile.RECORD_TYPE_VIDEO)
                        .mainViewMediaType(UCLOUD_RTC_SDK_MEDIA_TYPE_VIDEO.ordinal())
                        .VideoProfile(UCloudRtcSdkVideoProfile.UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480.ordinal())
                        .Average(UcloudRtcSdkRecordProfile.RECORD_UNEVEN)
                        .WaterType(UcloudRtcSdkRecordProfile.RECORD_WATER_TYPE_IMG)
                        .WaterPosition(UcloudRtcSdkRecordProfile.RECORD_WATER_POS_LEFTTOP)
                        .WarterUrl("http://urtc-living-test.cn-bj.ufileos.com/test.png")
                        .Template(UcloudRtcSdkRecordProfile.RECORD_TEMPLET_9)
                        .build();
                sdkEngine.startRecord(recordProfile);
                //如果主窗口不是当前推流用户，而是被订阅的用户
//                UCloudRtcSdkStreamInfo uCloudRtcSdkStreamInfo = mVideoAdapter.getStreamInfo(0);
//                if(uCloudRtcSdkStreamInfo != null){
//                    UcloudRtcSdkRecordProfile recordProfile = UcloudRtcSdkRecordProfile.getInstance().assembleRecordBuilder()
//                            .recordType(UcloudRtcSdkRecordProfile.RECORD_TYPE_VIDEO)
//                            .mainViewUserId(uCloudRtcSdkStreamInfo.getUId())
//                            .mainViewMediaType(uCloudRtcSdkStreamInfo.getMediaType().ordinal())
//                            .VideoProfile(UCloudRtcSdkVideoProfile.UCLOUD_RTC_SDK_VIDEO_PROFILE_640_480.ordinal())
//                            .Average(UcloudRtcSdkRecordProfile.RECORD_UNEVEN)
//                            .WaterType(UcloudRtcSdkRecordProfile.RECORD_WATER_TYPE_IMG)
//                            .WaterPosition(UcloudRtcSdkRecordProfile.RECORD_WATER_POS_LEFTTOP)
//                            .WarterUrl("http://urtc-living-test.cn-bj.ufileos.com/test.png")
//                            .Template(UcloudRtcSdkRecordProfile.RECORD_TEMPLET_9)
//                            .build();
//                    sdkEngine.startRecord(recordProfile);
//                }

//UCloudRtcSdkEventListener 
//录像开始回调
void onRecordStart(int code,String fileName);

//录像结束
sdkEngine.stopRecord();

//UCloudRtcSdkEventListener 结束回调
void onRecordStop(int code);
```    


## ** iOS **

## 2.1 iOS开始录制

```objectivec
  UCloudRtcRecordConfig *recordConfig = [UCloudRtcRecordConfig new];
  recordConfig.mainviewid = userId;  //主窗口位置用户id
  recordConfig.mimetype = 3;         //录制类型  1 音频 2 视频 3 音频+视频
  recordConfig.mainviewmt = 1;       //主窗口的媒体类型 1 摄像头 2 桌面
  recordConfig.bucket = @"urtc-test";//存储地址的名称
  recordConfig.region = @"cn-bj";    //所属的region
  recordConfig.watermarkpos = 1;     //水印的位置
  recordConfig.width = 360;          //录制视频的宽
  recordConfig.height = 480;         //录制视频的高
  recordConfig.isaverage = YES;      //是否均分
  recordConfig.waterurl = @"http://urtc-living-test.cn-bj.ufileos.com/test.png";//watertype 2时代表图片水印url 、watertype 3代表水印文字
  recordConfig.watertype = 1;        //1 (时间水印) 、 2 (图片水印) 、 3（文字水印)
  recordConfig.wtemplate = 9;        //模板
  [self.engine startRecord:recordConfig];   
```

```swift
  let recordConfig = UCloudRtcRecordConfig.init()
  recordConfig.mainviewid = userId;   //主窗口位置用户id
  recordConfig.mimetype = 3;          //录制类型  1 音频 2 视频 3 音频+视频
  recordConfig.mainviewmt = 1;        //主窗口的媒体类型 1 摄像头 2 桌面
  recordConfig.bucket = "urtc-test";  //存储地址的名称
  recordConfig.region = "cn-bj";      //所属的region
  recordConfig.watermarkpos = 1;      //水印的位置
  recordConfig.width = 360;           //录制视频的宽
  recordConfig.height = 480;          //录制视频的高
  recordConfig.isaverage = YES;       //是否均分
  recordConfig.waterurl = @"http://urtc-living-test.cn-bj.ufileos.com/test.png";//watertype 2时代表图片水印url 、watertype 3代表水印文字
  recordConfig.watertype = 1;         //1 (时间水印) 、 2 (图片水印) 、 3（文字水印)
  recordConfig.wtemplate = 9;         //模板
  self.engine?.startRecord(recordConfig)
```

## 2.2 iOS获取录制的文件地址

视频录制开始的回调方法会包含自动生成的视频录制文件存放地址，如下方式获取：

 ```objectivec
   -(void)uCloudRtcEngine:(UCloudRtcEngine *)manager startRecord:(NSDictionary *)recordResponse{
      [self.view makeToast:[NSString stringWithFormat:@"视频录制文件:%@",recordResponse[@"FileName"]] duration:3.0     position:CSToastPositionCenter];
    }
    
```

```swift
  func uCloudRtcEngine(_ manager: UCloudRtcEngine, startRecord recordResponse: [AnyHashable : Any]) {
        CBToast.showToastAction(message: NSString(format: "视频录制文件:%@", recordResponse["FileName"] as! CVarArg))
    }
    
 ```  

## 2.3 iOS停止录制

示例代码：    

```objectivec
    [self.manager stopRecord];
```

```swift
    self.manager?.stopRecord()
```

<!-- tabs:end -->


## 3. 开发注意事项

 - 录像需要混流时，可以指定主界面是哪个用户，垂直风格（大小布局）下，主界面是哪个用户的摄像头/共享屏幕，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`、`mainViewType`去实现。如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`、`mainViewType`。
 - 更多的录像的参数说明可以参照各个客户端的sdk API文档以及 [混流风格](urtc/cloudRecord/RecordLaylout)。 
 - 录制的文件大小，与录制时设置的码率相关。    
 	 - 以码率`1 Mbps`为例，录制1个小时，录制的文件大小约为 `1 Mbps/8 * 60 *60 s = 450 MB`。



