

# WEB SDK指南

 [Demo下载](https://github.com/ucloud/urtc-js-demo)

## 1、创建一个 URTC Client

### 1.1 使用 npm 安装

将 sdk 使用 ES6 语法作为模块引入。

1）使用 [npm](https://www.npmjs.com/) 或 [Yarn](https://yarnpkg.com/) 安装 [WEB SDK](https://github.com/ucloud/urtc-sdk-web):

```
npm install --save urtc-sdk
```

或

```
yarn add urtc-sdk
```

2）项目中引入并创建 client

```
import { Client } from 'urtc-sdk';

const client = new Client(appId, token); // 默认为直播模式（大班课），若为连麦模式（小班课）时，需要传入第三个参数 { type: 'rtc' }，更多配置见 sdk API 说明
```
>由于浏览器的安全策略对除 127.0.0.1 以外的 `HTTP` 地址作了限制，Web SDK 仅支持  `HTTPS` 协议  或者 `http://localhost（http://127.0.0.1）`，请勿使用  `HTTP` 协议 部署你的项目。

### 1.2 直接引入SDK

直接在页面中用 script 标签将 sdk 引入，此时会有全局对象 UCloudRTC

1）直接将 sdk 中 lib 目录下的 index.js 使用 script 标签引入

```JavaScript
<script type="text/javascript" src="index.js"><script>
```


2）使用全局对象 UCloudRTC 创建 client

```JavaScript
const client = new UCloudRTC.Client(appId, token);
```

> 注：创建 client 时传的 token 需要使用 AppId 和 AppKey 等数据生成，测试阶段，可临时使用  [sdk](https://github.com/ucloud/urtc-sdk-web)  提供的 generateToken 方法生成，但为保证 AppKey 不暴露于公网，在生产环境中强烈建议自建服务，由 [服务器按规则](/video/urtc/sdk/token) 生成 token 供 sdk 使用。

## 1.3 监听流事件

```JavaScript
client.on('stream-published', (stream) => {
    // 使用 HtmlMediaElement 播放媒体流。将流的 mediaStream 给 Video/Audio 元素的 srcObject 属性，即可播放，注意设置 autoplay 属性以支持视频的自动播放，其他属性请参见 [<video>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)
    htmlMediaElement.srcObject = stream.mediaStream;
}); // 监听本地流发布成功事件，在当前用户执行 publish 后，与服务器经多次协商，建立好连接后，会触发此事件

client.on('stream-subscribed', (stream) => {
    // 使用 HtmlMediaElement 播放媒体流
    htmlMediaElement.srcObject = stream.mediaStream;
}); // 监听远端流订阅成功事件，在当前用户执行 subscribe 后，与服务器经多次协商，建立好连接后，会触发此事件

client.on('stream-added', (stream) => {
    client.subscribe(stream.sid);
}); // 监听新增远端流事件，在远端用户新发布流后，服务器会推送此事件的消息。注：当刚进入房间时，若房间已有流，也会收到此事件的通知
```

## 1.4 加入一个房间，然后发布本地流

```JavaScript
client.joinRoom(roomId, userId, () => {
    client.publish();
}); // 在 joinRoom 的 onSuccess 回调函数中执行 publish 发布本地流
```

## 1.5 取消发布本地流或取消订阅远端流

```JavaScript
client.unpublish();
client.unsubscibe(streamId);
```

## 1.6 退出房间

```JavaScript
client.leaveRoom();
```
