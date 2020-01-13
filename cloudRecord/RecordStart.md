# 云端录制

<!-- tabs:start -->

# ** Web **

无需集成额外的SDK，通过以下方法，可以快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。


## 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [开通云端录制](video/urtc/cloudRecord/openRecord)。

## 开始录制音视频

示例代码：

```js
client.startRecording({
  bucket: string  // 必传，存储的 bucket, URTC 使用 UCloud 的 UFile 产品进行在存储，相关信息见控制台操作文档
  region: string  // 必传，存储服务所在的地域
  waterMark?: {
	  position?: 'left-top' | 'left-bottom' | 'right-top' | 'right-bottom' // 选传，指定水印的位置，前面四种类型分别对应 左上，左下，右上，右下，默认 'left-top'
	  type?: 'time' | 'image' | 'text' // 选传，水印类型，分别对应时间水印、图片水印、文字水印，默认为 'time'
	  remarks?:  string,   // 选传，水印备注，当为时间水印时，传空字符串，当为图片水印时，此处需为图片的 URL（此时必传），当为文字水印时，此处需为水印文字
	},
  mixStream?: {
	  uid?: string,        // 选传，指定某用户的流作为主画面，不传时，默认为当前开启录制的用户的流作为主画面
	  type?: 'screen' | 'camera',   // 选传，指定主画面使用的流的媒体类型（当同一用户推多路流时），不传时，默认使用 camera
	  width?: number,      // 选传，设置混流后视频的宽度，不传时，默认为 1280
	  height?: number,     // 选传，设置混流后视频的高度，不传时，默认为 720
	  template?: number,   // 选传，指定混流布局模板，可使用 1-9 对应的模板，默认为 1
	  isAverage?: boolean, // 选传，是否均分，均分对应平铺风格，不均分对应垂直风格，默认为 true
	}
}, function onSuccess(Record) {

	//开始录制成功返回信息：录制的文件的名称FileName和录制编号RecordId

}, function(Err) {

	//开始录制错误返回值

})
```


## 停止录制音视频

示例代码：

```js
client.stopRecording(function onSuccess() {

	//停止录制成功时执行的回调函数

}, function(Err) {

	//停止录制错误返回值
	
})
```  

## 开发注意事项

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均分模式、垂直模式下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。

更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](/video/urtc/cloudRecord/RecordLaylout)。 


# ** Windows **

无需集成额外的SDK，通过以下方法，可以快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。


## 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [开通云端录制](video/urtc/cloudRecord/openRecord)。


## 录制示例代码

```c++
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型
recordconfig.mMainviewuid = m_userid.data(); // 主画面
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD; // 录制等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;
recordconfig.mBucket = "your bucket";
recordconfig.mBucketRegion = "your bucket region";
recordconfig.mIsaverage = false; // 画面是否均分 不均分 均采用 1大几小格式 大画面在左 小画面在右
recordconfig.mWaterMarkType = UCLOUD_RTC_WATERMARK_TYPE_TIME;  // 水印类型
recordconfig.mWatermarkUrl = "hello urtc"; // 如果是文字水印为水印内容   如果是图片则为图片url 地址
recordconfig.mMixerTemplateType = 4; [混流模板](video/urtc/cloudRecord/RecordLaylout)
m_rtcengine->startRecord(recordconfig);
m_rtcengine->startRecord(recordconfig);

消息回调
//开启录制回调
virtual void onStartRecord (const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
``` 




## 开发注意事项

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均分模式、垂直模式下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。

更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](/video/urtc/cloudRecord/RecordLaylout)。 


# ** Android **


无需集成额外的SDK，通过以下方法，可以快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。



## 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [开通云端录制](video/urtc/cloudRecord/openRecord)。

> 录像目前只支持摄像头录制，不支持桌面录制。

## 录制示例代码

```js
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

## 开发注意事项

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均分模式、垂直模式下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。

更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](/video/urtc/cloudRecord/RecordLaylout)。 



# ** iOS **

无需集成额外的SDK，通过以下方法，可以快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。  

## 前提条件

开始录制之前，请确保开通录制服务，获取存储的`bucket`和存储服务所在的地域`region`。具体可参照 [开通云端录制](video/urtc/cloudRecord/openRecord)。    

## 开始录制

示例代码：    

```objective-c
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

## 获取录制的文件地址

视频录制开始的回调方法会包含自动生成的视频录制文件存放地址，如下方式获取：

 ```objective-c
   -(void)uCloudRtcEngine:(UCloudRtcEngine *)manager startRecord:(NSDictionary *)recordResponse{
      [self.view makeToast:[NSString stringWithFormat:@"视频录制文件:%@",recordResponse[@"FileName"]] duration:3.0     position:CSToastPositionCenter];
    }
    
```

```swift
  func uCloudRtcEngine(_ manager: UCloudRtcEngine, startRecord recordResponse: [AnyHashable : Any]) {
        CBToast.showToastAction(message: NSString(format: "视频录制文件:%@", recordResponse["FileName"] as! CVarArg))
    }
    
 ```  

## 停止录制

示例代码：    

```objective-c
    [self.manager stopRecord];
```

```swift
    self.manager?.stopRecord()
```

## 开发注意事项

> 需要特别注意的是，录像可以指定主界面是哪个用户，当非均分模式、垂直模式下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠`mainviewuid`去实现，如果是上述第一种情况，可以不指定，sdk自动获取，如果是第二种，就需要App SDK使用者拿到当前订阅的用户id，用这个id去设置录像的`mainviewuid`。

更多的录像的参数说明可以参照sdk API文档以及 [录制混流风格](/video/urtc/cloudRecord/RecordLaylout)。 

# ** Linux **

# ** macOS **

# ** Electron **

<!-- tabs:end -->

