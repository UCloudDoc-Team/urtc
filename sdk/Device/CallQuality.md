# 通话中质量监测

在实时音视频通话、互动直播中，可以通过获取统计数据，来监测通话质量。


<!-- tabs:start -->

## ** Web **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项


## ** Windows **

### 实现方法

```cpp

//网络评分回调
//@param uid 用户ID
//@param rtype 网络上下型类型
//@param Quality 评分
virtual void onNetworkQuality(const char* uid, eUCloudRtcNetworkQuality&rtype, eUCloudRtcQualityType& Quality) {}

```
### 示例代码

重载实现相关接口

### 开发注意事项

```cpp

UCLOUD_RTC_NETWORK_TX 上行
UCLOUD_RTC_NETWORK_RX 下行

//网络评分分为以下等级
typedef enum {
	//未知
	UCLOUD_RTC_QUALITY_UNKNOWN = 0, 
	//很坏
	UCLOUD_RTC_QUALITY_DOWN = 5,  
	//勉强能沟通但不顺畅
	UCLOUD_RTC_QUALITY_BAD = 4,  
	//用户主观感受有瑕疵但不影响沟通
	UCLOUD_RTC_QUALITY_POOR =  3, 
	// 用户主观感觉和 excellent 差不多
	UCLOUD_RTC_QUALITY_GOOD = 2, 
	//网络质量极好
	UCLOUD_RTC_QUALITY_EXCELLENT = 1, 
}eUCloudRtcQualityType; 

```
## ** Android **

### 实现方法

流建立后，接收服务端反馈的网络质量，根据抖动、延迟和丢包得出一个综合评价。

### 示例代码

```java
//userId:用户id streamType：流类型 streamType：媒体类型 quality：质量评价等级
        @Override
        public void onNetWorkQuality(String userId, UCloudRtcSdkStreamType streamType, UCloudRtcSdkMediaType mediaType, UCloudRtcSdkNetWorkQuality quality) {
            Log.d(TAG, "onNetWorkQuality: userid: " + userId + "streamType: " + streamType + "mediatype : "+ mediaType + " quality: " + quality);
        }

```    
### 开发注意事项

UCloudRtcSdkNetWorkQuality是一个枚举类型，参数说明如下：

```java
    /**
     * 质量未知
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_UNKNOWN,
    /**
     * 质量极好，基本无网络抖动
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_EXCELLENT,
    /**
     * 质量较好，相对于正常，延迟和丢包更低一点
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_GOOD,
    /**
     * 质量正常，有一定的延迟和丢包，但均可接受的范围内
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_NORMAL,
    /**
     * 质量差，勉强能沟通
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_BAD,
    /**
     * 质量糟糕,基本不能沟通
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_TERRIBLE,
    /**
     * 基本断开，完全无法沟通
     */
    U_CLOUD_RTC_SDK_NET_WORK_QUALITY_DOWN;

```  



## ** iOS **

### 实现方法

``` objc
/// 网络质量检测
/// @param manager UCloudRtcEngine
/// @param userId 用户id
/// @param txQuality 上行质量（枚举值）
/// @param rxQuality 下行质量（枚举值）
- (void)uCloudRtcEngine:(UCloudRtcEngine *)manager networkQuality:(NSString *)userId txQuality:(UCloudRtcNetworkQuality)txQuality rxQuality:(UCloudRtcNetworkQuality)rxQuality {
    NSString *txQualityStr;
    NSString *rxQualityStr;
    switch (txQuality) {
        case UCloudRtcNetworkQualityUnknown:
            txQualityStr = @"未知";
            break;
        case UCloudRtcNetworkQualityExcellent:
            txQualityStr = @"优秀";
            break;
        case UCloudRtcNetworkQualityGood:
            txQualityStr = @"良好";
            break;
        case UCloudRtcNetworkQualityPoor:
            txQualityStr = @"一般";
            break;
        case UCloudRtcNetworkQualityPoorer:
            txQualityStr = @"较差";
            break;
        case UCloudRtcNetworkQualityPoorest:
            txQualityStr = @"糟糕";
            break;
        case UCloudRtcNetworkQualityDisconnect:
            txQualityStr = @"连接断开";
            break;
        default:
            break;
    }
   switch (rxQuality) {
        case UCloudRtcNetworkQualityUnknown:
            rxQualityStr = @"未知";
            break;
        case UCloudRtcNetworkQualityExcellent:
            rxQualityStr = @"优秀";
            break;
        case UCloudRtcNetworkQualityGood:
            rxQualityStr = @"良好";
            break;
        case UCloudRtcNetworkQualityPoor:
            rxQualityStr = @"一般";
            break;
        case UCloudRtcNetworkQualityPoorer:
            rxQualityStr = @"较差";
            break;
        case UCloudRtcNetworkQualityPoorest:
            rxQualityStr = @"糟糕";
            break;
        case UCloudRtcNetworkQualityDisconnect:
           rxQualityStr = @"连接断开";
           break;
        default:
            break;
    }
    NSLog(@"userId:%@, 上行网络质量:%@，下行网络质量:%@",userId, txQualityStr, rxQualityStr);
}
```

### 开发注意事项

balabala……  



<!-- tabs:end -->
