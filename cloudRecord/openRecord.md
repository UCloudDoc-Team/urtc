# 开通云端录制

URTC 云端录制，是URTC针对实时音视频研发的录制服务。  

无需额外的SDK，通过简单的操作方法，帮助开发者快速、灵活的实现录制服务，实现一对一、一对多的音视频通话或直播的录制。  

## 功能说明：  

  - 单流录制，支持录制指定某个用户的音频流和视频流  
  - 合流录制，录制房间内，所有用户的音视频，合流录制为一个音视频文件  
  - 支持多种混流风格 
  - 录制支持添加时间水印、文字水印、图片水印
  - 支持存储在Ucloud的对象存储Ufile的指定存储空间  
  
## 1、开通录制服务
  
  [登录控制台](https://passport.ucloud.cn/?service=https://console.ucloud.cn/#login)，实时音视频 URTC 服务的应用下，开通录制服务。  
  
  ![ ](/images/record/openRecord.png)
  
## 2、配置录制文件的存储的路径  
  
开通录制服务后，需要新建录制的存储路径。    
  
![ ](/images/record/newBucket.png)
	 
 - 已经配置了对象存储的情况下，可以直接选择配置的存储空间，配置完毕即可。  

![ ](/images/record/newBucket2.png) 
	 
 - 配置完毕后，看到存储空间状态为可用。
 
![ ](/images/record/creatBucket4.png)

## 3、新建对象存储空间

 - 在没有对象存储的情况下，需要去对象存储Ufile 下，创建一个存储；再按照上述步骤配置存储空间。   

![ ](/images/record/creatBucket.png) 

 >了解更多对象存储的内容，可以  [查看这里](https://docs.ucloud.cn/storage_cdn/ufile/quick/quick_start)  。  


### 3.1 创建存储空间

![ ](/images/record/creatBucket1.png)

### 3.2 创建令牌

![ ](/images/record/lingpai.png)

创建令牌时，注意选择已经创建的存储空间。  

![ ](/images/record/lingpai2.png)

选择后，能看到令牌对应的存储空间。注意选择令牌的有效期、权限。

![ ](/images/record/lingpai4.png)

确认后，保存即可。


	 
  
  
