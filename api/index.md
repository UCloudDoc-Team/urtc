# API文档

实时音视频URTC SDK 支持多个客户端，不同客户端的API方法不完全一致。    
这里介绍Web，Windows，Android，iOS/macOS这几个平台的核心功能 API，帮助你快速了解各个平台之间的差异。    

| 方法      | Web | Windows | Android | iOS |
|-|-|-|-|-|
| 初始化参数 | [client](https://github.com/ucloud/urtc-sdk-web#client)  | [Windows初始化](https://docs.ucloud.cn/urtc/sdk/VideoStart)  | [Android初始化](https://docs.ucloud.cn/urtc/sdk/VideoStart)  | [iOS初始化](https://docs.ucloud.cn/urtc/sdk/VideoStart)  |
| 加入房间   | [joinRoom](https://github.com/ucloud/urtc-sdk-web#client-joinroom)  | [joinChannel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-joinChannel)  | joinChannel  | joinRoom  |
| 离开房间   | [leaveRoom](https://github.com/ucloud/urtc-sdk-web#client-leaveroom)  | [leaveChannel](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-leaveChannel)  |   |   |
| 发布流     | [publish](https://github.com/ucloud/urtc-sdk-web#client-publish)  | [publish](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-publish)  |   |   |
| 监听事件   | [on](https://github.com/ucloud/urtc-sdk-web#client-on)  | [regRtcEventListener](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-regRtcEventListener)  |   |   |
| 订阅流     | [subscribe](https://github.com/ucloud/urtc-sdk-web#client-subscribe)  | [subscribe](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-subscribe)  |   |   |
| 销毁实例   | **NA**  | [destroy](https://github.com/ucloud/urtc-win-demo/tree/master/doc#class-destroy)  |   |   |
