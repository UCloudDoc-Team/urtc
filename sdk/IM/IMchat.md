# IM聊天
<!-- {docsify-ignore-all} -->
<!-- tabs:start -->

# ** Web **

balabala……  

# ** Windows **

# ** Linux **

# ** Android **


# ** iOS **

  [现在下载IM SDK](http://urtcsdk.cn-bj.ufileos.com/UCloudIMSdk_ios.zip)   
    
## 1. 导入UCloudIMSdk

```
#import <UCloudIMSdk/UCloudIMSdk.h>
```
## 2. 初始化UCloudIMEngine

```
    UCloudIMEngine *imEngine = [UCloudIMEngine sharedImEngine];
    [imEngine cutOffConnect];
    imEngine.delegate = self;
    imEngine.userId = userId;
    imEngine.roomId = roomId;
    imEngine.appId = appId;
```
## 3. 加入房间
```
    ....
    [self.imEngine joinRoom:parameters completionHandler:^(NSData * _Nullable data, NSError * _Nullable error) {}];
```

## 4. 发送自定义消息

```
    ....
    [self.imEngine pushCustomContent:parameters completionHandler:^(NSData * _Nullable data, NSError * _Nullable error) {}];
```

# ** macOS **

# ** Electron **

<!-- tabs:end -->

