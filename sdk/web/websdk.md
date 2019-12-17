

# WEB SDK指南

 [Demo下载](https://github.com/ucloud/urtc-js-demo)

## 1、创建一个 URTC Client

### 1.1 使用 npm 安装

将 sdk 使用 ES6 语法作为模块引入。

1）使用 [npm](https://www.npmjs.com/) 或 [Yarn](https://yarnpkg.com/) 安装 WEB SDK:

```
npm install --save urtc-sdk
```

或

```
yarn add urtc-sdk
```

2）项目中引入并创建 client

 [下载WEB SDK](https://github.com/ucloud/urtc-sdk-web)

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

## 1.5 服务端录制

#### 前提条件
开始录制之前，请确保开通录制服务，具体可参照 [开通云端录制](https://docs.ucloud.cn/video/urtc/cloudRecord/openRecord)。


#### 开始录制音视频
示例代码：

```
client.startRecording({
  bucket: string  // 必传，存储的 bucket, URTC 使用 UCloud 的 UFile 产品进行在存储，相关信息见控制台操作文档
  region: string  // 必传，存储服务所在的地域
  waterMark?: {
	  position?: 'left-top' | 'left-bottom' | 'right-top' | 'right-bottom' // 选传，指定水印的位置，前面四种类型分别对应 左上，左下，右上，右下，默认 'left-top'
	  type?: 'time' | 'image' | 'text' // 选传，水印类型，分别对应时间水印、图片水印、文字水印，默认为 'time'
	  remarks?:  string,   // 选传，水印备注，当为时间水印时，传空字符串，当为图片水印时，此处需为图片的 URL（此时必传），当为文字水印时，此处需为水印文字
	},
  mixStream?: {
	  uid?: string,        // 选传，指定某用户的流作为主画面，不传时，默认为当前开启录制的用户的流作为主画面
	  type?: 'screen' | 'camera',   // 选传，指定主画面使用的流的媒体类型（当同一用户推多路流时），不传时，默认使用 camera
	  width?: number,      // 选传，设置混流后视频的宽度，不传时，默认为 1280
	  height?: number,     // 选传，设置混流后视频的高度，不传时，默认为 720
	  template?: number,   // 选传，指定混流布局模板，可使用 1-9 对应的模板，默认为 1
	  isAverage?: boolean, // 选传，是否均分，均分对应平铺风格，不均分对应垂直风格，默认为 true
	}
}, function onSuccess(Record) {

	//录制成功返回信息：录制的文件的名称FileName和录制编号RecordId

}, function(Err) {

	//录制错误返回值

})
```

> 注：录制可以指定主界面是哪个用户。当非均衡模式的情况下，主界面是哪个用户，哪个用户就占据大窗口。主界面用户可以是客户端推流用户，也可以是客户端订阅用户，这个参数只要靠mainviewuid去实现。如果当前是客户端推流用户，可以不指定mainviewuid，sdk会自动获取，如果当前是客户端订阅的用户，就需要SDK使用者拿到当前订阅的用户id，用这个id去设置录制的mainviewuid 更多的录制的参数说明可以参照[录制混流风格](https://github.com/UCloudDocs/urtc/blob/master/cloudRecord/RecordLaylout.md)

#### 停止录制音视频
示例代码：

```
client.stopRecording(function onSuccess() {

	//停止录制成功时执行的回调函数

}, function(Err) {

	//停止录制错误返回值
	
})
```


## 1.6 取消发布本地流或取消订阅远端流

```JavaScript
client.unpublish();
client.unsubscibe(streamId);
```

## 1.7 退出房间

```JavaScript
client.leaveRoom();
```



