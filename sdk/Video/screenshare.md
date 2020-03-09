# 屏幕共享

屏幕共享，可以把客户端的屏幕，以视频的方式分享给其他人。

屏幕共享，支持分享整个屏幕、分享应用、分享浏览器窗口。

Windows、安卓客户端的屏幕共享，已经在SDK里集成。具体参照客户端的快速集成SDK及相关的API接口文档。

<!-- tabs:start -->

# ** Web **

* Chrome及firefox 屏幕共享

> 注：在开始屏幕共享前，请确保集成Web端SDK，详见[快速集成SDK](https://github.com/ucloud/urtc-sdk-web/blob/master/Manual.md) 。

## 无插件屏幕共享

Chrome 72及以上版本，可以使用无插件屏幕分享。   
在 Chrome、firefox 上共享屏幕时 调用`publish`方法把`video`字段设为 `false`， `screen` 字段设为 `true` 即可。   

```js
client.publish(
  {
     audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
  },
  succ => {
    console.log("add screen stream success ", succ);
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

## 使用屏幕共享插件

chrome 72及72版本以上无需下载插件，72版本以下则需要下载[URTC屏幕共享插件](http://urtcsdk.cn-bj.ufileos.com/URTC-screen-extention.zip)。

打开你的 Chrome 浏览器，点击屏幕右上方的扩展按钮，选择 更多工具 > 扩展程序， 打开开发者模式 > 加载已解压的扩展程序 > 选择 解压的 URTC屏幕共享插件文件夹，即可完成安装

安装 URTC 提供的 Chrome 屏幕共享插件 ，并获取插件的 `extensionId`，在 publish 的时候填入 `extensionId`。

```js
client.publish(
  {
    audio: boolean, // 必填，指定是否使用麦克风设备
    video: boolean, // 必填，指定是否使用摄像头设备
    screen: boolean, // 必填，指定是否为桌面共享，注意，video 和 screen 不可同时为 true
    extensionId?: string // 选填，指定使用的Chrome插件的 extensionId，使用Chrome屏幕共享插件时必填
  },
  succ => {
    console.log("add screen stream success ", succ);
  },
  err => {
    console.log("add screen stream  failure ", err);
  }
);
```

* audio 属性建议设置为 false，避免订阅端收到的两路流中都有音频，导致回声，建议音频只推一路流，防止出现回音。
* 在本地共享的时候，本地流的 Client 不要订阅本地的媒体流，否则会增加时长计费。
* 创建屏幕共享流的时候，video/audio 必须设置为 false。



<!-- tabs:end -->
