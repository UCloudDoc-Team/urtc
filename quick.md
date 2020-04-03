# 快速上手

通过集成URTC SDK，可以从零开始，快速搭建出实时音视频通信平台，可以应用于语音和视频社交、在线教育和培训、远程医疗、在线会议、直播等多种业务场景。 

集成URTC SDK之前，需要在UCLOUD官网控制台创建URTC应用。

## 1． 登录UCLOUD控制台

在UCLOUD官网，[【登录控制台】](https://passport.ucloud.cn/?service=https://console.ucloud.cn/#login)。  

> 使用URTC服务之前，首先需要[注册账号](https://passport.ucloud.cn/#register) 并且完成 [实名认证](https://docs.ucloud.cn/identity_verification/README) 。  

## 2．创建URTC应用

> 每个账号最大支持创建5个URTC应用，需要创建更多URTC应用，请联系客户经理增加配额。

可以通过2种方法：控制台、API创建URTC应用。    

### 2.1  控制台创建URTC应用

 - 在控制台，【全部产品】-【视频服务】-【实时音视频】，找到【我的应用】  

![](/images/creat_app.png) 

 - 点击创建应用，输入应用名称，确定后保存。  
 
![](/images/creat_app_2.png) 

 - 确定后，自动生成AppID、AppKey。  
 
![](/images/app_go.png) 

 - 绑定AppID及AppKey到您的应用中即可开始使用。
 
### 2.2  API创建URTC应用

通过 [创建URTC 应用](https://docs.ucloud.cn/api/urtc-api/create_urtc_app)的API，也可以创建URTC应用。具体调用API的方法，请查看[API文档](https://docs.ucloud.cn/api/summary/README)。

## 3. 下载URTC SDK  

可以通过2种方法：控制台、集成文档种下载URTC SDK。    

### 3.1  控制台下载SDK

 - 在控制台，【全部产品】-【视频服务】-【实时音视频】-【文档及SDK】内，下载SDK。  

![](/images/download_SDK.png) 
  
>下载SDK的同时，还可以下载demo源码。使用中有任何问题，都可以通过GitHub提issue，反馈给我们。

### 3.2 文档中下载SDK

 - 可以在[快速集成SDK](urtc/sdk/VideoStart) 中，下载SDK。
