# 快速上手

通过集成URTC SDK，可以从零开始，搭建出实时音视频通信平台，可以应用于语音和视频社交、在线教育和培训、远程医疗、在线会议、直播等多种业务场景。 

首先需要在UCLOUD官网控制台注册账户，并完成实名认证；然后创建URTC应用。

## 1． 注册账户并完成实名认证

在UCLOUD官网，[【登录控制台】](https://passport.ucloud.cn/?service=https://console.ucloud.cn/#login)。  

> 使用URTC产品之前，必须先 [注册账号](https://passport.ucloud.cn/#register) 并且完成 [实名认证](https://docs.ucloud.cn/account/identity_verification/overview) 。  

## 2．下载URTC SDK  

### 2.1  在控制台中下载SDK

 - 在控制台，【全部产品】-【视频服务】-【实时音视频】-【文档及SDK】内，下载SDK。  

![](/images/download_SDK.png) 
  
>下载SDK的同时，还可以下载demo源码。使用中有任何问题，都可以通过GitHub提issue，反馈给我们。

### 2.2 在集成SDK文档中下载SDK

 - 可以在[快速集成SDK](/video/urtc/sdk/VideoStart) 中，下载SDK。


## 3．创建URTC应用

> 单个账号最大支持5个URTC应用，需要创建更多应用请联系客户经理增加配额。

### 3.1  在控制台中创建URTC应用

 - 找到【我的应用】  

![](/images/creat_app.png) 

 - 点击创建应用，输入应用名称，确定后保存。  
 
![](/images/creat_app_2.png) 

 - 确定后，自动生成AppID、AppKey。  
 
![](/images/app_go.png) 

 - 绑定AppID及AppKey到您的应用中即可开始使用。
 
### 3.2  通过API创建URTC应用

通过 [创建URTC 应用](https://docs.ucloud.cn/api/urtc-api/create_urtc_app)的API，可以创建URTC应用。

具体调用API的方法，请查看[API文档](https://docs.ucloud.cn/api/summary/overview)。
