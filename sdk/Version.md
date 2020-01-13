# 版本说明

<!-- tabs:start -->

# ** web **

## 1.4.6版

该版本发布于 2020-01-03。

1. 开放私有化部署时针对房间服务器部署与否两种场景的设置
2. 添加 types 文件，支持用 typescript 调用 sdk 编写代码
3. 新增获取本地浏览器支持的音视频编码格式的功能
4. 修复 iOS safari, 360 等浏览器无法发布的问题，已经支持的浏览器：
 - 支持Chrome 60及以上版本
 - 支持Safari 11及以上版本
 - 支持Firefox 56及以上版本
 - 支持opera 50及以上版本
 - 支持QQ浏览器 10及以上版本
 - 支持360安全浏览器 10及以上版本
 - 支持360极速浏览器 12及以上版本
5. 修复日志上报参数错误的问题
6. 修复远端 mute 视频时，本地展示画面不黑屏，停留在最后一帧的问题
7. 完善/修正 API 文档



# ** Windows **

## 1.6.1版

该版本发布于2019-12-3。  

1、加入大班课功能，支持超大规模会议功能    

使用如下：

``` c++ 
必须在最开始调用
m_rtcengine->setChannelType(UCLOUD_RTC_CHANNEL_TYPE_BROADCAST);
注意：
大班课中用户只能拥有订阅或者发布一种权限
如果m_rtcengine->setStreamRole(UCLOUD_RTC_USER_STREAM_ROLE_BOTH);
SDK会默认把权限设置为UCLOUD_RTC_USER_STREAM_ROLE_SUB
```
2、添加自定义编码（最大分辨率到1080p(1920*1080)）    

``` c++ 
virtual void setVideoProfile(eUCloudRtcVideoProfile profile) = 0;
变更为
virtual void setVideoProfile(eUCloudRtcVideoProfile profile, tUCloudVideoConfig& videoconfig) = 0;
tUCloudVideoConfig 用来定义自己编码分辨率
```
3、录制功能增加更多录制模板以及水印 调用示例如下：    

``` c++ 
tUCloudRtcRecordConfig recordconfig;
recordconfig.mMainviewmediatype = UCLOUD_RTC_MEDIATYPE_VIDEO; // 主画面类型 video screen
recordconfig.mMainviewuid = m_userid.data(); // 主画面用户id
recordconfig.mProfile = UCLOUD_RTC_RECORDPROFILE_SD;//录制输出等级
recordconfig.mRecordType = UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO;//录制媒体内容
recordconfig.mWatermarkPos = UCLOUD_RTC_WATERMARKPOS_LEFTTOP;//水印位置
recordconfig.mBucket = "urtc-test";  // ufile bucket
recordconfig.mBucketRegion = "cn-bj"; // ufile region
recordconfig.mIsaverage = false; // 画面是否均分 不均分 均采用 1大几小格式 大画面在左 小画面在右
recordconfig.mWaterMarkType = UCLOUD_RTC_WATERMARK_TYPE_TIME;  // 水印类型
recordconfig.mWatermarkUrl = "hello urtc"; // 如果是文字水印为水印内容   如果是图片则为图片url 地址
recordconfig.mMixerTemplateType = 4; [混流模板](http:https://github.com/UCloudDocs/urtc/blob/master/cloudRecord/RecordLaylout.md)
m_rtcengine->startRecord(recordconfig);
```



<!-- tabs:end -->
