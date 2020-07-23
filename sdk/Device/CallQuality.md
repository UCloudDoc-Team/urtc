# 通话中质量监测

在实时音视频通话、互动直播中，可以通过获取统计数据，来监测通话质量。


<!-- tabs:start -->

## ** Web **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  

## ** Windows **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  

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

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  



<!-- tabs:end -->
