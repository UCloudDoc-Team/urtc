# macOS SDK 指南

## 1. 下载资源

  - 可以下载 Demo、SDK、API文档    
  
    [现在下载](https://github.com/ucloud/urtc-mac-demo.git)

## 2. 开发语言以及系统要求

  - 系统版本：macOS 10.0；           


## 3. 开发环境  

  - Xcode 9.0及以上版本； 

  - Apple开发证书或个人账号；    


## 4. 搭建开发环境  

### 4.1. 得到动态库

下载SDK,得到的UCloudRtcSdk\_mac.framework为动态库；    

### 4.2. 创建新的工程

使用XCode创建一个新的工程UCloudRtcSdk-mac-demo；   

![](/images/sdk/MACOS/4.2.png)

### 4.3. 加入动态库到工程中

将已下载的动态库UCloudRtcSdk\_mac.framework加入到UCloudRtcSdk-mac-demo工程中Embedded Binaries；    

![](/images/sdk/MACOS/4.3.png)

### 4.4. 打开Xcode

将TARGETS>GENERAL>Deployment Target 设置为10.10及以上；

![](/images/sdk/MACOS/4.4.png)

### 4.5. 编辑info.plist，申请摄像头、麦克风权限

Privacy - Camera Usage Description    
Privacy - Microphone Usage Description    

![](/images/sdk/MACOS/4.5.png)

### 4.6. 打开网络请求相关权限

![](/images/sdk/MACOS/4.6.png)

### 4.7. 集成成功

按照上述步骤完成UCloudRtcSdk-mac-demo的前期SDK集成准备之后，执行编译
Commond + B，提示Build Success，表示SDK集成成功。  

## 5. 初始化

建议在初始化 App 的同时，初始化 SDK。  

### 5.1. 导入 SDK 头文件  

```
<UCloudRtcSdk_mac/UCloudRtcSdk_mac.h>
```

### 5.2. 设置 userId 和 roomId，获取AppID;  

```
UCloudRtcEngine *engine = [[UCloudRtcEngine alloc]
initWithUserId:userId appId:appId roomId:roomId token:@""]];
```

务必要设置代理对象，并实现代理回调方法，设置代理对象失败，会导致 App 收不到相关回调。

```
engine.delegate = self;
```

### 5.3. 调用接口初始化

使用之前需要对SDK进行相关设置，如果不设置，系统将会采用默认值。  

```
self.engineMode = UCloudRtcEngineModeTrival; 默认为测试模式
self.engine.isAutoPublish = YES;//加入房间后将自动发布本地音视频 默认为YES
self.engine.isAutoSubscribe = YES;//加入房间后将自动订阅远端音视频 默认为YES
self.engine.isDesktop = NO;//发布桌面或者摄像头 默认为NO:摄像头 YES:桌面
```

## 6. 建立通话

### 6.1. 加入房间

```
[self.engine joinRoomWithcompletionHandler:^(NSData *data, NSUrlResponse *response, NSError error) {
}];

```

### 6.2. 发布本地流  

1）自动发布模式下，joinRoom成功后，随即发布本地流；      

2）发布过程中可以监听以下事件获取发布状态，根据状态调用渲染或其他接口即可。      

```
- (void)uCloudRtcEngine:(UCloudRtcEngine *)manager didChangePublishState:(UCloudRtcEnginePublishState)publishState {
    switch (publishState) {
        case UCloudRtcEnginePublishStateUnPublish:
            self.isConnected = NO;
            break;
        case UCloudRtcEnginePublishStatePublishing: {
            [self.bottomButton setTitle:@"正在发布..." forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishSucceed:{
            self.isConnected = YES;
            [self.view makeToast:@"发布成功" duration:1.5 position:CSToastPositionCenter];
            [self.bottomButton setTitle:@"发布成功" forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStateRepublishing: {
            [self.bottomButton setTitle:@"正在重新发布..." forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishFailed: {
            self.isConnected = NO;
            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
        }
            break;
        case UCloudRtcEnginePublishStatePublishStoped: {
            self.isConnected = NO;
            [self.view makeToast:@"发布已停止" duration:1.5 position:CSToastPositionCenter];
            [self.bottomButton setTitle:@"开始发布" forState:UIControlStateNormal];
        }
            break;
        default:
            break;
    }
}
```

### 6.3. 订阅远程流  

1）自动订阅模式下，joinRoom成功后，即可订阅远程流；    

2）订阅成功，在回调事件中调用渲染接口即可。  

```
-(void)uCloudRtcEngine:(UCloudRtcEngine *)channel didSubscribe:(UCloudRtcStream *)stream{
     [self reloadVideos];
}
```

### 6.4. 离开房间

```
[self.engine leaveRoom];
```

### 6.5. 编译、运行，开始体验吧！
