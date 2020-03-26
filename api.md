# API文档

<!-- tabs:start -->

# ** Web **

URTC  Web SDK包含以下方法、类或对象：

* [一、Client 类](#client)
* [二、getDevices 方法](#getdevices)
* [三、getSupportProfileNames 方法](#getsupportprofilenames)
* [四、getSupportedCodec 方法](#getsupportedcodec)
* [五、isSupportWebRTC 方法](#issupportwebrtc)
* [六、deviceDetection 方法](#devicedetection)
* [七、version 属性](#version)
* [八、generateToken 方法](#generateToken)
* [九、Logger 对象](#logger)
* [十、setServers 方法](#setservers)

> 注： 想要了解使用此 SDK 的简单步骤，请查看 [使用说明](https://github.com/ucloud/urtc-sdk-web/blob/master/Manual.md)

---

<a name='client'></a>

## 一、Client 类

Client 类包含以下方法：

* [构造函数 - 创建客户端](#client-constructor)
* [joinRoom 方法 - 加入房间](#client-joinroom)
* [leaveRoom 方法 - 离开房间](#client-leaveroom)
* [publish 方法 - 发布本地流](#client-publish)
* [unpublish 方法 - 取消发布本地流](#client-unpublish)
* [subscribe 方法 - 订阅远端流](#client-subscribe)
* [unsubscribe 方法 - 取消订阅远端流](#client-unsubscribe)
* [on 方法 - 绑定事件处理函数](#client-on)
* [off 方法 - 解绑事件处理函数](#client-off)
* [muteAudio 方法 - 禁用音频轨道](#client-muteaudio)
* [unmuteAudio 方法 - 启用音频轨道](#client-unmuteaudio)
* [muteVideo 方法 - 禁用视频轨道](#client-mutevideo)
* [unmuteVideo 方法 - 启用视频轨道](#client-unmutevideo)
* [startRecording 方法 - 开启服务端录制](#client-startrecording)
* [stopRecording 方法 - 结束服务端录制](#client-stoprecording)
* [getUser 方法 - 获取当前用户信息](#client-getuser)
* [getUsers 方法 - 获取所有远端用户信息](#client-getusers)
* [getStream 方法 - 获取某条流信息](#client-getstream)
* [getLocalStreams 方法 - 获取所有本地流信息](#client-getlocalstreams)
* [getRemoteStreams 方法 - 获取所有远端流信息](#client-getremotestreams)
* [~~getStreams 方法 - 已废弃~~](#client-getstreams)
* [getMediaStream 方法 - 获取某条流对应的媒体流](#client-getmediastream)
* [~~getLocalMediaStream 方法 - 已废弃~~](#client-getlocalmediastream)
* [~~getRemoteMediaStream 方法 - 已废弃~~](#client-getremotemediastream)
* [getMicrophones 方法 - 获取麦克风设备信息](#client-getmicrophones)
* [getCameras 方法 - 获取摄像头设备信息](#client-getcameras)
* [getLoudspeakers 方法 - 获取扬声器设备信息](#client-getloudspeakers)
* [setVideoProfile 方法 - 设置视频属性](#client-setvideoprofile)
* [switchDevice 方法 - 切换音视频输入设备](#client-switchdevice)
* [switchScreen 方法 - 切换屏幕共享](#client-switchscreen)
* [switchImage 方法 - 切换图片推送](#client-switchimage)
* [getAudioVolume 方法 - 获取音频音量](#client-getaudiovolume)
* [setAudioVolume 方法 - 设置音频音量](#client-setaudiovolume)
* [getAudioStats 方法 - 获取音频状态](#client-getaudiostats)
* [getVideoStats 方法 - 获取视频状态](#client-getvideostats)
* [getNetworkStats 方法 - 获取网络状态](#client-getnetworkstats)
* [preloadEffect 方法 - 预加载音效文件](#client-preloadeffect)
* [unloadEffect 方法 - 卸载音效文件](#client-unloadeffect)
* [playEffect 方法 - 开始播放音效](#client-playeffect)
* [pauseEffect 方法 - 暂停播放音效](#client-pauseeffect)
* [resumeEffect 方法 - 恢复播放音效](#client-resumeeffect)
* [stopEffect 方法 - 停止播放音效](#client-stopeffect)
* [setEffectVolume 方法 - 设置播放音效的音量](#client-seteffectvolume)
* [snapshot 方法 - 截屏](#client-snapshot)
* [startPreviewing 方法 - 开启预览](#client-startpreviewing)
* [stopPreviewing 方法 - 停止预览](#client-stoppreviewing)
* [~~deviceDetection 方法 - 已转移~~](#client-devicedetection)
* [replaceTrack 方法 - 替换音频轨道或视频轨道](#client-replacetrack)
* [startMix 方法 - 开始录制或转推](#client-startmix)
* [stopMix 方法 - 结束录制或转推](#client-stopmix)
* [queryMix 方法 - 查询录制或转推](#client-querymix)
* [addMixStreams 方法 - 为录制或转推添加需要混合的流](#client-addmixstreams)
* [removeMixStreams 方法 - 删除录制或转推中混合的流](#client-removemixstreams)

<a name="client-constructor"></a>

### 1. 构造函数

用于创建一个 URTC Client 对象，示例代码：

```
new Client(AppId, Token, ClientOptions);
```

#### 参数说明

- AppId: string 类型, 必传，可从 UCloud 控制台查看

- Token: string 类型, 必传，需按规则生成，测试阶段，可使用 [generateToken](#generateToken) 临时生成

- ClientOptions: object 类型, 选传，类型说明如下

```
{
  type?: "rtc"|"live",  // 选填，设置房间类型，有两种 "live" 和 "rtc" 类型可选 ，分别对应直播模式和连麦模式，默认为 rtc
  role?: "pull" | "push" | "push-and-pull",   // 选填，设置用户角色，可设 "pull" | "push" | "push-and-pull" 三种角色，分别对应拉流、推流、推+拉流，默认为 "push-and-pull"，特别地，当房间类型为连麦模式（rtc）时，此参数将被忽视，会强制为 "push-and-pull"，即推+拉流
  codec?: "vp8"|"h264", // 选填，设置视频编码格式，可设 "vp8" 或 "h264"，默认为 "h264"
}
```

<a name="client-joinroom"></a>

### 2. joinRoom 方法

加入房间，示例代码：

```
client.joinRoom(RoomId, UserId, onSuccess, onFailure)
```

#### 参数说明

- RoomId: string 类型，必传，房间号

- UserId: string 类型，必传，用户ID

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(Users, Streams) {}
```

函数参数 Users 为返回值，User 类型的数组，代表当前房间内已有的其他用户的信息，User 类型说明见 [User](#user)；函数参数 Streams 为返回值，Stream 类型的数组，代表当前房间内其他用户正在发布的流。Stream 类型说明见 [Stream](#stream)

> 注：当加入房间成功后，当前房间内已有的其他用户的信息以及正在发布的流，都会分别由 `user-added` 和 `stream-added` 事件再进行通知。如需订阅正在发布的流，建议在 `stream-added` 事件函数中统一处理，此处 `onSuccess` 函数的参数 `Users` 和 `Streams` 数据仅用于展示。

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="client-leaveroom"></a>

### 3. leaveRoom 方法

离开房间，示例代码：

```
client.leaveRoom(LeaveRoomOptions, onSuccess, onFailure)
```

#### 参数说明

- LeaveRoomOptions，object 类型，选传，类型说明如下

```
{
  keepRecording: boolean  // 是否保持服务端录制，默认不保持。使用场景：课堂管理员开启房间内的流进行服务端录制后，不需要等待课堂结束即可直接退出房间，并使在房间内的流继续录制。
}
```

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="client-publish"></a>

### 4. publish 方法

发布本地流，自 1.4.0 版本开始支持同时发布两条流（且摄像头，屏幕共享各一条，不可同时为同一类），示例代码：

```
client.publish(PublishOptions, onFailure)
```

#### 参数说明

- PublishOptions: object 类型，选传，类型说明如下

```
{
  audio: boolean          // 必填，指定是否使用麦克风设备
  video: boolean          // 必填，指定是否使用摄像头设备
  facingMode?: FacingMode // 选填，在移动设备上，可以设置该参数选择使用前置或后置摄像头，其中，FacingMode 为 'user'（前置摄像头）或 'environment'（后置摄像头），注：1. 请务必确定是在移动设备上设置该参数，否则可能会报 'OverConstrainedError［无法满足要求错误]' 的错误；2. 当在设备上使用前置摄像头时，设备（如苹果设备，以及没有设置摄像头为 "自拍镜像" 的 Android 设备）本地显示的图像是左右相反的（以 Y 轴对称），此时可为 video 元素添加样式 'transform: rotateY(180deg);' 来指定视频在本地显示的时候做镜像翻转。
  screen: boolean         // 必填，指定是否为屏幕共享，audio, video, screen 不可同时为 true，更不可同时为 false
  microphoneId?: string   // 选填，指定使用的麦克风设备的ID，可通过 getMicrophones 方法查询获得该ID，不填时，将使用默认麦克风设备
  cameraId?: string       // 选填，指定使用的摄像头设备的ID，可以通过 getCameras 方法查询获得该ID，不填时，将使用默认的摄像头设备
  extensionId?: string    // 选填，指定使用的 Chrome 插件的 extensionId，可使 72 以下版本的 Chrome 浏览器进行屏幕共享。
  mediaStream? MediaStream  // 选填，允许用户发布自定义的媒体流
}
```

> 对于屏幕共享各浏览器兼容性，请参见 [getDisplayMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getDisplayMedia) 。
> 特别地，使用 Chrome 浏览器屏幕共享 Tab（浏览器标签）页时，有分享 Tab 页中音频功能（分享弹出框左下脚勾选，Chrome 74 及以上版本可不用安装插件）。
>
> 此外，可使用 audio, video, screen 的配置组合来满足不同场景，如 audio = true, video = false, screen = true 时，可发布麦克风的音频以及屏幕共享的视频流；audio = false，video = true, screen = true 时，可发布屏幕共享中的音频（当屏幕共享有音频时）以及摄像头采集的视频流；audio = false, video = false, screen = true 时，可发布屏幕共享中的音频以及视频（当屏幕共享有音频时）流。

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="device-error"></a>

> 可能的错误信息有：
>
> - AbortError［中止错误］ 尽管用户和操作系统都授予了访问设备硬件的权利，而且未出现可能抛出NotReadableError异常的硬件问题，但仍然有一些问题的出现导致了设备无法被使用。
> - NotAllowedError［拒绝错误］ 用户拒绝了当前的浏览器实例的访问请求；或者用户拒绝了当前会话的访问；或者用户在全局范围内拒绝了所有媒体访问请求。
> - NotFoundError［找不到错误］ 找不到满足请求参数的媒体类型。
> - NotReadableError［无法读取错误］ 尽管用户已经授权使用相应的设备，操作系统上某个硬件、浏览器或者网页层面发生的错误导致设备无法被访问。
> - OverConstrainedError［无法满足要求错误］ 指定的要求无法被设备满足。


<a name="client-unpublish"></a>

### 5. unpublish 方法

取消发布本地流，示例代码：

```
client.unpublish(StreamId, onSuccess, onFailure)
```

#### 参数说明
- StreamId: string 类型，选传，不传时，若仅有一条本地流，那么该流将被取消发布，若有两条本地流，那么最早发布的那条本地流将被取消发布

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(Stream) {}
```

函数参数 Stream 为返回值，object 类型，为流信息，类型说明见 [Stream](#stream)

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="client-subscribe"></a>

### 6. subscribe 方法

订阅远端流，，示例代码：

```
client.subscribe(StreamId, onFailure)
```

#### 参数说明

- StreamId: string 类型，必传，为需要订阅的远端流的 sid

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-unsubscribe"></a>

### 7. unsubscribe 方法

取消订阅远端流，示例代码：

```
client.unsubscribe(StreamId, onSuccess, onFailure)
```

#### 参数说明

- StreamId: string 类型，必传，为需要订阅的远端流的 sid

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(Stream) {}
```

函数参数 Stream 为返回值，object 类型，为流信息，类型说明见 [Stream](#stream)

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-on"></a>

### 8. on 方法

给事件绑定监听函数，示例代码：

```
client.on(EventType, Listener)
```

#### 参数说明

- EventType: string 类型， 必传，目前有 'user-added' | 'user-removed' |
  'stream-added' |'stream-removed' | 'stream-published' | 'stream-subscribed' |
  'mute-video' | 'unmute-video' | 'mute-audio' | 'unmute-audio' | 'screenshare-stopped' |
  'connection-state-change' | 'kick-off' | 'network-quality' 这些事件可绑定监听函数
- Listener: function 类型，事件监听函数

  - 当事件类型为 'user-added' | 'user-removed' | 'kick-off' 时，可用 `function Listener(User) {}` 类型的函数，其中函数的参数类型见 [User](#user)
  - 当事件类型为 'stream-added' | 'stream-removed' | 'stream-published' | 'stream-subscribed' | 'mute-video' | 'unmute-video' | 'mute-audio' | 'unmute-audio' | 'screenshare-stopped' 时，可用 `function Listener(Stream) {}` 类型的函数，其中函数的参数类型见 [Stream](#stream)
  - 当事件类型为 'connection-state-change' 时，可用 `function Listener(States) {}` 类型的函数，函数的参数为 `{previous: State, current: State}`，其中 State 为 'OPEN' | 'CONNECTING' | 'CLOSING' | 'RECONNECTING' | 'CLOSED' 四种类型之一。
  - 当事件类型为 'network-quality' 时，可用 `function Listener(Stats) {}` 类型的函数，函数的参数为 `{uplink: Quality, downlink: Quality}`，其中，uplink 代表上行网络质量，downlink 代表下行网络质量，其值 Quality 为字符串，是 '0' | '1' | '2' | '3' | '4' | '5' | '6' 几种类型之一。
    - '0': 网络质量未知
    - '1': 网络质量优秀
    - '2': 网络质量良好
    - '3': 网络质量一般
    - '4': 网络质量较差
    - '5': 网络质量糟糕
    - '6': 网络连接断开


#### 事件名解释：

事件名 | 描述
:--: | :--
user-added | 有其他用户加入房间
user-removed | 有其他用户离开房间
stream-added | 有其他用户发布了一条流，当收到此事件通知时，可调用 subscribe 方法订阅该流
stream-removed | 有其他用户取消发布了一条流
stream-published | 本地流已发布，可获取该流对应的媒体流来进行播放
stream-subscribed | 远端流已订阅，可获取该流对应的媒体流来进行播放
mute-video | 流的 video 被 mute
unmute-video | 流的 video 被取消 mute
mute-audio | 流的 audio 被 mute
unmute-audio | 流的 audio 被取消 mute
screenshare-stopped | 屏幕共享已被手动停止，当收到此事件通知时，需调用 unpublish 方法取消发布本地流
connection-state-change | 当 URTC client 与服务器的连接状态变化时，会由此事件通知。特别地，当因网络问题导致连接断开时，sdk 将在1分钟内尝试自动重连，此时将收到内容为 `{previous: "OPEN", current: "RECONNECTING"}` 的通知，表示开始重连，若重连成功，将收到内容为 `{previous: "RECONNECTING", current: "OPEN"}` 的通知，若不能重连成功，将收到内容为 `{previous: "RECONNECTING", current: "CLOSED"}` 的通知，表示重连失败，此时需要依次调用 leaveRoom 和 joinRoom 以重新进入房间。
kick-off | 当前用户被踢出了房间。URTC 限制了多设备同时登录，同一用户（userId）不可同时在多处加入房间，即当同一用户（userId）分别在利用两个设备（譬如电脑、手机等）先后加入房间时，前一个加入房间的设备将在后一个加入房间时收到此事件的通知，此时业务层可提示用户。
network-quality | 报告本地用户的上下行网络质量。每 2 秒触发一次，报告本地用户当前的上行和下行网络质量。`该功能目前处于实验阶段，网络质量评分仅供参考`


<a name="client-off"></a>

### 9. off 方法

解除绑定事件的监听函数，示例代码：

```
client.off(EventType, Listener)
```

#### 参数说明

- EventType: 参见 on 方法
- Listener: 为调用 on 方法时绑定的监听函数

<a name="client-muteaudio"></a>

### 10. muteAudio 方法

关闭流的音频，示例代码：

```
const result = client.muteAudio(StreamId)
```

#### 参数说明

- StreamId: string 类型，选传，指流的 ID

> 注：StreamId 不传时，为 mute 第一条发布流的音频，并会通知到其他用户；传时，为 mute StreamId 对应的发布或订阅流的音频，若为订阅流时，只是影响到本地订阅的流的音频，并不是指远端流推送或不推送音频。

#### 返回值说明

- result: boolean 类型，成功时为 true，失败时为 false


<a name="client-unmuteaudio"></a>

### 11. unmuteAudio 方法

启用流的音频，示例代码：

```
const result = client.unmuteAudio(StreamId)
```

#### 参数说明

- StreamId: string 类型，选传，指流的 ID

> 注：StreamId 不传时，为 unmute 第一条发布流的音频，并会通知到其他用户；传时，为 unmute StreamId 对应的发布或订阅流的音频，若为订阅流时，只是影响到本地订阅的流的音频，并不是指远端流推送或不推送音频。

#### 返回值说明

- result: boolean 类型，成功时为 true，失败时为 false


<a name="client-mutevideo"></a>

### 12. muteVideo 方法

关闭流的视频，示例代码：

```
const result = client.muteVideo(StreamId)
```

- StreamId: string 类型，选传，指流的 ID

> 注：StreamId 不传时，为 mute 第一条发布流的视频，并会通知到其他用户；传时，为 mute 对应的发布或订阅流的视频，若为订阅流时，只是影响到本地订阅的流的视频，并不是指远端流推送或不推送视频。

#### 返回值说明

- result: boolean 类型，成功时为 true，失败时为 false


<a name="client-unmutevideo"></a>

### 13. unmuteVideo 方法

启用流的视频，示例代码：

```
const result = client.unmuteVideo(StreamId)
```

- StreamId: string 类型，选传，指流的 ID

> 注：StreamId 不传时，为 unmute 发布流的视频，并会通知到其他用户；传时，为 unmute 对应的发布或订阅流的视频，若为订阅流时，只是影响到本地订阅的流的视频，并不是指远端流推送或不推送视频。

#### 返回值说明

- result: boolean 类型，成功时为 true，失败时为 false


<a name="client-startrecording"></a>

### 14. startRecording 方法

开始录制音视频，示例代码：

```
client.startRecording(RecordOptions, onSuccess, onFailure)
```

#### 参数说明

- RecordOptions: object 类型，必传，录制的配置信息，类型说明如下

```
{
  bucket: string  // 必传，存储的 bucket, URTC 使用 UCloud 的 UFile 产品进行在存储，相关信息见控制台操作文档
  region: string  // 必传，存储服务所在的地域
  uid?: string,                         // 选传，指定某用户的流作为主画面，不传时，默认为当前开启录制的用户的流作为主画面
  mainViewType?: 'screen' | 'camera',   // 选传，指定主画面使用的流的媒体类型（当同一用户推多路流时），不传时，默认使用 camera
  mixStream?: MixStreamOptions // 选传，混流的相关配置，无混流时，不用填写
  waterMark?: WaterMarkOptions // 选传，水印的相关配置，不需要添加水印时，不用填写
  relay?: RelayOptions         // 选传，转推相关配置，不需要转推时，不用填写
}
```

WaterMarkOptions: object 类型，选传，添加的水印相关配置，类型说明如下

```
{
  position?: 'left-top' | 'left-bottom' | 'right-top' | 'right-bottom' // 选传，指定水印的位置，前面四种类型分别对应 左上，左下，右上，右下，默认 'left-top'
  type?: 'time' | 'image' | 'text' // 选传，水印类型，分别对应时间水印、图片水印、文字水印，默认为 'time'
  remarks?:  string,   // 选传，水印备注，当为时间水印时，传空字符串，当为图片水印时，此处需为图片的 URL（此时必传），当为文字水印时，此处需为水印文字
}
```

MixStreamOptions: object 类型，选传，混流相关配置，类型说明如下

```
{
  width?: number,      // 选传，设置混流后视频的宽度，不传时，默认为 1280
  height?: number,     // 选传，设置混流后视频的高度，不传时，默认为 720
  template?: number,   // 选传，指定混流布局模板，可使用 1-9 对应的模板，默认为 1
  isAverage?: boolean, // 选传，是否均分，均分对应平铺风格，不均分对应垂直风格，默认为 true
}
```

> 注：关于混流风格, 请参见详细的模板说明 [录制混流风格](https://github.com/UCloudDocs/urtc/blob/master/cloudRecord/RecordLaylout.md)

RelayOptions: object 类型，选传，转推相关配置，类型说明如下

```
{
  time?: number,        // 转推开启时间的Unix时间戳（单位：秒），不填时，将默认使用当前时间的Unix时间戳
  fragment: number,     // 切片大小（单位：秒），默认 60s
}
```

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(Record) {}
```

函数参数 Record 为返回值，object 类型，为流信息，类型说明如下

```
{
  FileName: string  // 录制到的文件的名称
  RecordId: string  // 录制 ID
}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-stoprecording"></a>

### 15. stopRecording 方法

停止录制音视频，示例代码：

```
client.stopRecording(onSuccess, onFailure)
```

#### 参数说明

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getuser"></a>

### 16. getUser 方法

获取本地用户的信息，示例代码：

```
const result = client.getUser()
```

#### 返回值说明

- result: User 类型，类型说明如下

<a name='user'></a>

User:

```
{
  uid: string   // 为用户ID
}
```


<a name="client-getusers"></a>

### 17. getUsers 方法

获取当前加入房间的远端用户的信息，示例代码：

```
const result = client.getUsers()
```

#### 返回值说明

- result: User 类型的数组，User 类型说明见 [User](#user)


<a name="client-getstream"></a>

### 18. getStream 方法

获取单条发布（本地）/订阅（远端）流的信息，示例代码：

```
const result = client.getStream(StreamId)
```

#### 参数说明

- StreamId: string 类型，选传，流的 ID，当不传时，默认返回第一条发布流（当有两条发布流时）

#### 返回值说明

- result: Stream 类型或 undefined（未找到对应流），Stream 类型说明如下

<a name='stream'></a>

Stream:

```
{
  sid: string                     // 流ID
  uid: string                     // 对应的用户的ID
  type: 'publish'|'subscribe'     // 流类型，分别为 publish 和 subscribe 两种，
  video: boolean                  // 是否包含音频
  audio: boolean                  // 是否包含视频
  muteAudio: boolean              // 音频轨道是否禁用
  muteVideo: boolean              // 视频轨道是否禁用
  mediaType?: 'camera'|'screen'   // 流的媒体类型，目前存在两种媒体类型 'camera' 及 'screen'，同一用户可发布的各类型的流只能存在一个，以此来区分不同媒体类型的发布/订阅流
  mediaStream?: MediaStream       // 使用的媒体流，可用 HTMLMediaElement 进行播放，此属性的值可能为空，当流被正常发布或订阅流，此值有效
}
```


<a name="client-getlocalstreams"></a>

### 19. getLocalStreams 方法

获取所有发布（本地）流的信息（ 1.4.0 及以上版本支持），示例代码：

```
const result = client.getLocalStreams()
```

#### 返回值说明

- result: Stream 类型的数组，Stream 类型说明见 [Stream](#stream)

<a name="client-getremotestreams"></a>

### 20. getRemoteStreams 方法

获取所有订阅流（远端流）的信息（ 1.4.0 及以上版本支持），示例代码：

```
const result = client.getRemoteStreams()
```

#### 返回值说明

- result: Stream 类型的数组，Stream 类型说明见 [Stream](#stream)

<a name="client-getstreams"></a>

### ~~getStreams 方法 - 已废弃~~

获取订阅流（远端流）的信息，1.4.0 及以上版本请使用 [getRemoteStreams](#client-getremotestreams)


<a name="client-getmediastream"></a>

### 21. getMediaStream 方法

获取发布（本地）/ 订阅（远端）流对应的媒体流（ 1.4.0 及以上版本支持），获取后，可通过 HtmlMediaElement（如：[video](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)）进行播放，示例代码：

```
const result = client.getMediaStream(StreamId)
```

#### 参数说明

- StreamId: string 类型，选传，流的 ID，当不传时，默认返回第一条发布流（当有两条发布流时），传时，返回 StreamId 对应的发布或订阅流的的媒体流

#### 返回值说明

- result: MediaStream 类型，类型说明见 [MediaStream](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream)


<a name="client-getlocalmediastream"></a>

### ~~getLocalMediaStream 方法 - 已废弃~~

获取发布流对应的媒体流，1.4.0 及以上版本请使用 [getMediaStream](#client-getmediastream)


<a name="client-getremotemediastream"></a>

### ~~getRemoteMediaStream 方法 - 已废弃~~

获取订阅流对应的媒体流，1.4.0 及以上版本请使用 [getMediaStream](#client-getmediastream)


<a name="client-getmicrophones"></a>

### 22. getMicrophones 方法

获取麦克风设备，示例代码：

> 注：若站点未经过用户授权浏览器使用麦克风设备，会弹出提示要求用户进行授权

```
client.getMicrophones(onSuccess, onFailure)
```

#### 参数说明

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(MediaDeviceInfos) {}
```

函数参数 MediaDeviceInfos 为返回值，为 MediaDeviceInfo 类型的数组，点击 [MediaDeviceInfo](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo) 查看 MediaDeviceInfo 详情


- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getcameras"></a>

### 23. getCameras 方法

获取摄像头设备，示例代码：

> 注：若站点未经过用户授权浏览器使用摄像头设备，会弹出提示要求用户进行授权

```
client.getCameras(onSuccess, onFailure)
```

#### 参数说明

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(MediaDeviceInfos) {}
```

函数参数 MediaDeviceInfos 为返回值，为 MediaDeviceInfo 类型的数组，点击 [MediaDeviceInfo](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo) 查看 MediaDeviceInfo 详情


- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getloudspeakers"></a>

### 24. getLoudspeakers 方法

获取音响/声音输出设备，示例代码：

> 注：若站点未经过用户授权浏览器使用音频设备，会弹出提示要求用户进行授权

```
client.getLoudspeakers(onSuccess, onFailure)
```

#### 参数说明

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(MediaDeviceInfos) {}
```

函数参数 MediaDeviceInfos 为返回值，为 MediaDeviceInfo 类型的数组，点击 [MediaDeviceInfo](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo) 查看 MediaDeviceInfo 详情


- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-setvideoprofile"></a>

### 25. setVideoProfile 方法

设置视频的 profile（通过getSupportProfileNames获取到视频质量的值，不设置时，默认为 "640\*480"）限制 client 使用的视频大小、帧率、带宽等，setVideoProfile须在publish之前设置。示例代码：

```
client.setVideoProfile(Profile, onSuccess, onFailure)
```

#### 参数说明

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-switchdevice"></a>

### 26. switchDevice 方法

当发布（本地麦克风、摄像头）流已经发布，可通过此方法在不中断当前发布的情况下，用指定的音视频设备采集的音视频流代替正在发布的音视频流，示例代码：

```
client.switchDevice(SwitchDeviceOptions, onSuccess, onFailure)
```

#### 参数说明

- SwitchDeviceOptions: object 类型，必传，详细类型说明如下

```
{
  streamId?: string       // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  type: 'audio' | 'video' // 必填，指定音频或视频设备
  deviceId: string        // 必填，设备 ID
}
```

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```
- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

> 可能的错误信息有：[参见](#device-error)


<a name="client-switchscreen"></a>

### 27. switchScreen 方法

当屏幕共享流已经发布，可通过此方法在不中断当前发布的情况下，用屏幕共享来代替正在发布的屏幕共享流，示例代码：

```
client.switchScreen(StreamId, onSuccess, onFailure)
```

#### 参数说明

- StreamId: string 类型，选传，不传时，为第一条发布流（请确保第一条发布为屏幕共享流）

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```
- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-switchimage"></a>

### 28. switchImage 方法

当本地流已经发布，可通过此方法在不中断当前发布的情况下，用静态图片来代替正在发布的视频流，示例代码：

```
client.switchImage(SwitchImageOptions, onSuccess, onFailure)
```

#### 参数说明

- SwitchImageOptions: object 类型，必传，详细类型说明如下

```
{
  streamId?: string       // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  file?: File             // 选填，指（图片）文件，更多请参考下面的备注
  filePath?: string       // 选填，指图片文件的网络路径（URL)，支持以下图片格式：PNG，JPEG 以及浏览器支持的其他图片格式，注：当图片文件为其他站点的网络文件时，可能会有跨域访问问题
}
```

> 注：
> 1. file 和 filePath 两者不可同时为空，至少填一项。特别地，若都填时，将优先使用 file 的值。
> 2. file 请参考 [File 对象](https://developer.mozilla.org/zh-CN/docs/Web/API/File)，一般可通过 `<input type="file" accept="image/*"></input>` 来上传来获取

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess() {}
```
- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getaudiovolume"></a>

### 29. getAudioVolume 方法

获取音频的音量，返回值范围 [0,100]，示例代码：

```
client.getAudioVolume(StreamId)
```

#### 参数说明

- StreamId: string 类型，选传，本地或远端流的 ID 即 [Stream](#stream) 的 sid 属性值，当不传时，默认获取第一条本地流的音量大小

<a name="client-setaudiovolume"></a>

### 30. setAudioVolume 方法

设置音频的音量，可设置的音量范围 [0,100]，示例代码：

```
client.setAudioVolume(AudioVolumeOptions, callback)
```

#### 参数说明

- AudioVolumeOptions: object 类型，必传，详细类型说明如下

```
{
  streamId?: string   // 选填，发布/订阅流的 ID，不填时，为第一条发布流
  element?: HTMLMediaElement // 播放媒体流的 DOM 元素，当需要设置音量的流为发布（本地）流时，不填，为订阅（远端）流，必填
  volume: number      // 必填，音量大小，取值范围 [0, 100]
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息


<a name="client-getaudiostats"></a>

### 31. getAudioStats 方法

获取流的音频状态，示例代码：

```
client.getAudioStats(StreamId, onSuccess, onFailure)
```

#### 参数说明

- StreamId: string 类型，选传，本地或远端流的 ID 即 [Stream](#stream) 的 sid 属性值，当不传时，默认获取第一条本地流的音频状态
  
- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(AudioStats) {}
```
函数参数 AudioStats 为返回值，为 object 类型，类型说明如下：

```
{
  br: number        // 码率
  lostpre: number   // 丢包率
  vol: number       // 声音大小
  mime: string      // 编码格式，固定为 opus
}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getvideostats"></a>

### 32. getVideoStats 方法

获取流的视频状态，示例代码：

```
client.getVideoStats(StreamId, onSuccess, onFailure)
```

#### 参数说明

- StreamId: string 类型，选传，本地或远端流的 ID 即 [Stream](#stream) 的 sid 属性值，当不传时，默认获取第一条本地流的视频状态
  
- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(VideoStats) {}
```
函数参数 VideoStats 为返回值，为 object 类型，类型说明如下：

```
{
  br: number        // 码率
  lostpre: number   // 丢包率
  frt: number       // 帧率
  w: number         // 视频宽度
  h: number         // 视频高度
  mime: string      // 编码格式，'vp8' 或 'h264'
}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息


<a name="client-getnetworkstats"></a>

### 33. getNetworkStats 方法

获取流的网络状态，示例代码：

```
client.getNetworkStats(StreamId, onSuccess, onFailure)
```

#### 参数说明

- StreamId: string 类型，可选，本地或远端流的 ID 即 [Stream](#stream) 的 sid 属性值，当不传时，默认获取第一条本地流的网络状态
  
- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(NetworkStats) {}
```
函数参数 NetworkStats 为返回值，为 object 类型，类型说明如下：

```
{
  rtt: number   //  往返时延，单位 ms
}
```

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="client-preloadeffect"></a>

### 34. preloadEffect 方法

预加载音效资源，示例代码：

```
client.preloadEffect(EffectId, FilePath, callback)
```

#### 参数说明

- EffectId: number 类型，必传，指音效资源 ID，须唯一，用于区分不同的音效资源

- FilePath: string 类型，必传，指音效文件的路径（URL)，支持以下音频格式：MP3，AAC 以及浏览器支持的其他音频格式。此外，音效文件不应过大，否则可能会影响通信的流畅性，注：当音效文件为其他站点的网络文件时，可能会有跨域访问问题

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

<a name="client-unloadeffect"></a>

### 35. unloadEffect 方法

卸载音效资源，示例代码：：

```
client.unloadEffect(EffectId)
```

#### 参数说明

- EffectId: number 类型，必传，指音效资源 ID，须唯一，用于区分不同的音效资源

<a name="client-playeffect"></a>

### 36. playEffect 方法

播放音效，示例代码：

```
client.playEffect(EffectOptions, callback)
```

#### 参数说明

<a name='effectoptions'></a>

- EffectOptions: object 类型，必传，详细类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
  filePath?: string   // 选填，音效文件的路径，当音效文件已经使用 preloadEffect 进行预加载后，可不填此项
  loop?: boolean      // 选填，是否循环播放音效，默认不循环
  playTime?: number   // 选填，音效从 playTime 秒处开始播放，默认为0，即从头开始
  replace?: boolean   // 选填，是否替换当前音轨，即只使用音效，不混音，默认不替换
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

<a name="client-pauseeffect"></a>

### 37. pauseEffect 方法

暂停播放音效，示例代码：

```
client.pauseEffect(EffectOptions, callback)
```

#### 参数说明

- EffectOptions: object 类型, 必传，详细的类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

<a name="client-resumeeffect"></a>

### 38. resumeEffect 方法

恢复播放音效，示例代码：

```
client.resumeEffect(EffectOptions, callback)
```

#### 参数说明

- EffectOptions: object 类型, 必传，详细的类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息


<a name="client-stopeffect"></a>

### 39. stopEffect 方法

停止播放音效，示例代码：

```
client.stopEffect(EffectOptions, callback)
```

#### 参数说明

- EffectOptions: object 类型, 必传，详细的类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

<a name="client-seteffectvolume"></a>

### 40. setEffectVolume 方法

设置正在播放的音效的音量大小，示例代码：

```
client.setEffectVolume(EffectVolumeOptions, callback)
```

#### 参数说明

- EffectVolumeOptions: object 类型, 必传，详细的类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  effectId: number    // 必填，音效资源 ID
  volume: number      // 必填，音量大小，取值范围 [0, 100]
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

<a name="client-snapshot"></a>

### 41. snapshot 方法

可将指定的发布（本地）/订阅（远端）流截屏用于页面展示，或下载截屏图片，示例代码：

```
client.snapshot(SnapshotOptions, onSuccess, onFailure);
```

> 注：为保证 API 的易用性，此 API 自 1.4.0 版本进行了重新设计，由于无法做到向前（ 1.3.7 - 1.3.10 版本）兼容，请使用 1.3.7 - 1.3.10 版本的用户调整调用方式。

#### 参数说明

- SnapshotOptions: object 类型，选传，详细的类型说明如下

```
{
  streamId?: string             // 选填，发布/订阅流的 ID，不填时，为第一条发布流，
  download?: boolean 或 string  // 选填，是否要下载图片，或指定下载图片的文件名，传 true 时，可将截屏下载到本地（文件名自动生成），传非空字符串时，将会以该字符串命名下载时保存到本地的图片名，不传或传 false 或空字符串时，都将不下载图片
}
```

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(ImgString) {}
```

ImgString: string 类型，是图片转化的 base64 编码的 [Data URLs](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/data_URIs)，可将其赋值给 Image 元素 - 详见 [HTMLImageElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement) 的 src 属性。


- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

<a name="client-startpreviewing"></a>

### 42. startPreviewing 方法

启动预览，示例代码：

```
client.startPreviewing(DeviceOptions, onSuccess, onFailure);
```

#### 参数说明

- DeviceOptions: object 类型，选传，详细的类型说明如下

```
{
  audio: boolean          // 必填，指定是否使用麦克风设备
  video: boolean          // 必填，指定是否使用摄像头设备
  facingMode?: FacingMode // 选填，在移动设备上，可以设置该参数选择使用前置或后置摄像头，其中，FacingMode 为 'user'（前置摄像头）或 'environment'（后置摄像头）
  microphoneId?: string   // 选填，指定使用的麦克风设备的ID，可通过 getMicrophones 方法查询获得该ID，不填时，将使用默认麦克风设备
  cameraId?: string       // 选填，指定使用的摄像头设备的ID，可以通过 getCameras 方法查询获得该ID，不填时，将使用默认的摄像头设备
}
```

- onSuccess: function 类型，选传，方法调用成功时执行的回调函数，函数说明如下

```
function onSuccess(result) {}
```

- result: MediaStream 类型，类型说明见 [MediaStream](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream)，可通过 HtmlMediaElement（如：[video](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video)）进行播放

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```

Err 为错误信息

> 可能的错误信息有：[参见](#device-error)


<a name="client-stoppreviewing"></a>

### 43. stopPreviewing 方法

停止预览，示例代码：

```
client.stopPreviewing();
```

<a name="client-devicedetection"></a>

### ~~deviceDetection 方法 - 已转移~~

此 API 已转移至 UCloudRTC 中，详见 [UCloudRTC.deviceDetection](#devicedetection)


<a name="client-replacetrack"></a>

### 44. replaceTrack 方法

替换发布流的音频轨道或视频轨道，可在保持发布流的发布状态下，切换音频或视频，示例代码：

```
client.replaceTrack(ReplaceTrackOptions, callback)
```

#### 参数说明

- ReplaceTrackOptions: object 类型, 必传，详细的类型说明如下

```
{
  streamId?: string   // 选填，发布（本地）流的 ID，不填时，为第一条发布流
  track: MediaStreamTrack   // 必填，需要替换的新的音轨或视轨，MediaStreamTrack 参见下面注释
  retain?: boolean    // 选填，是否需要保持被替换的老的音轨或视轨可用，一般情况下，如果后面需要切换回老的音轨或视轨，建议保持其可用，否则可不用保持
}
```

> MediaStreamTrack 类型，类型说明见 [MediaStreamTrack](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaStreamTrack)

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, OldTrack) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

OldTrack 为返回值，MediaStreamTrack 类型，不为空时，值为被替换的音频轨道或视频轨道

<a name="client-startmix"></a>

### 45. startMix 方法

开启录制或转推，示例代码：

```
client.startMix(MixOptions, callback)
```

#### 参数说明

- MixOptions: object 类型, 必传，详细的类型说明如下

```
{
  type?: MixType  // 选传，MixType 为 'relay' | 'record' | 'relay-and-record' | 'update-config' 其中之一，分别代表 '转推' | '录制' | '录制并转推' | '更改设置'，不传时，默认为 'record' 录制。
  bucket?: string  // 当 type 为 '录制' 和 '录制并转推时'，必传，存储的 bucket, URTC 使用 UCloud 的 UFile 产品进行在存储，相关信息见控制台操作文档
  region?: string  // 当 type 为 '录制' 和 '录制并转推时'，必传，存储服务所在的地域

  pushURL?: string[]          // 当 type 为 '转推' 和 '录制并转推时'，必传，为转推的 URL 的列表
  layout?: MixLayoutOptions   // 选传，录制/转推后的视频的布局设置，类型说明见下面的 MixLayoutOptions 类型，不传时，将使用默认值
  audio?: MixAudioOptions     // 选传，录制/转推后的音频的设置，类型说明见下面的 MixAudioOptions 类型，不传时，将使用默认值
  video?: MixVideoOptions     // 选传，录制/转推后的视频的设置，类型说明见下面的 MixVideoOptions 类型，不传时，将使用默认值

  width?: number      // 选传，录制转推后的视频的带宽，不传时，默认为 1280
  height?: number     // 选传，录制转推后的视频的调试，不传时，默认为 720
  backgroundColor?: BackgroundColorOptions  // 选传，背景色，类型说明见下面的 BackgroundColorOptions 类型，不传时，默认为 #000 黑色

  waterMark?: WaterMarkOptions  // 选传，水印设置，类型说明见下面的 WaterMarkOptions 类型，不传时将不添加水印

  streams?: MixStream[]         // 选传，指定需要混合的流，类型说明见下面的 MixStream 类型
}
```

<a name="mixlayoutoptions"></a>

MixLayoutOptions 类型，类型说明

```
{
  type: MixLayoutType           // MixLayoutType 为 'flow' | 'main' | 'custom'，分别代表：1 流式(均分)布局；2 讲课模式，主讲人占大部分屏幕，其他人小屏居于右侧或底部；3 自定义布局，默认为 'flow'
  custom?: Object[]             // type 为 'custom'时，自定义布局填在custom里，格式参照RFC5707 Media Server Markup Language (MSML)
  mainViewUId?: string          // type 为 'main' 时，指定某用户的流为主画面，默认为当前用户
  mainViewType?: MainViewType   // 主画面类型，'screen' | 'camera'，默认为用户发布的 'camera'
}
```

<a name="mixaudiooptions"></a>

MixAudioOptions 类型，类型说明

```
{
  codec: MixAudioCodec    // 音频的编码格式，MixAudioCodec 为 'aac'，不传时默认为 'aac'
}
```

<a name="mixvideooptions"></a>

MixVideoOptions 类型，类型说明

```
{
  codec: MixVideoCodec    // 视频的编码格式，MixVideoCodec 为 'h264' | 'h265' 其中之一，默认为 'h264'
  quality?: H264Quality   // 视频质量，当 codec 为 h264 时，此项起作用，H264Quality 为 'B' | 'CB' | 'M' | 'E' | 'H' 其中之一，默认为 'CB'
  frameRate?: number      // 视频码率，默认为 15
  bitRate?: number        // 视频比特率，默认为 500
}
```

<a name="backgroundcoloroptions"></a>

BackgroundColorOptions 类型，类型说明

```
{
  r: number   // r 通道值，默认为 0
  g: number   // g 通道值，默认为 0
  b: number   // b 通道值，默认为 0
}
```

<a name="watermarkoptions"></a>

WaterMarkOptions: object 类型，选传，添加的水印相关配置，类型说明如下

```
{
  position?: 'left-top' | 'left-bottom' | 'right-top' | 'right-bottom' // 选传，指定水印的位置，前面四种类型分别对应 左上，左下，右上，右下，默认 'left-top'
  type?: 'time' | 'image' | 'text' // 选传，水印类型，分别对应时间水印、图片水印、文字水印，默认为 'time'
  remarks?:  string,   // 选传，水印备注，当为时间水印时，传空字符串，当为图片水印时，此处需为图片的 URL（此时必传），当为文字水印时，此处需为水印文字
}
```

<a name="mixstream"></a>

MixStream: object 类型，选传，添加的水印相关配置，类型说明如下

```
{
  uid: string,        // 用户 ID，指定需要混合的流的用户ID（远端用户）
  mediaType: string   // 流的媒体类型，指定需要混合的流的媒体类型，有 'camera' 和 'screen' 两种可选，默认为 'camera'
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, Result) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

Result 为返回值，[MixResult 类型](#mixresult)，执行失败时，此值为空，执行成功时，此值为执行结果

<a name="mixresult"></a>

MixResult: object 类型，类型说明如下

```
{
  MixId: string         // 混流 ID
  FileName?: string     // 混流文件名
  Type?: MixType        // 混流类型，MixType 为 'relay' | 'record' | 'relay-and-record' | 'update-config'，注：queryMix 操作时会返回此项
  PushURL?: string[]    // 转推的 URL 列表，注：stopMix 操作时会返回此项
}
```

<a name="client-stopmix"></a>

### 46. stopMix 方法

关闭录制或转推，示例代码：

```
client.stopMix(StopMixOptions, callback)
```

#### 参数说明

- StopMixOptions: object 类型, 必传，详细的类型说明如下

```
{
  type?: MixType      // 选传，MixType 为 'relay' | 'record' | 'relay-and-record' 其中之一，分别代表 '转推' | '录制' | '录制并转推'，不传时，默认为 'record'
  pushURL?: String[]  // 字符串数组，若 type 为 relay 或 relay-and-record 时，可用此参数指定需要停止转推的 URL，或留空停止对所有 URL 的转推
}
```

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, Result) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

Result 为返回值，[MixResult 类型](#mixresult)，执行失败时，此值为空，执行成功时，此值为执行结果

<a name="client-querymix"></a>

### 47. queryMix 方法

查询录制或转推，示例代码：

```
client.queryMix(callback)
```

#### 参数说明

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, Result) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

Result 为返回值，[MixResult 类型](#mixresult)，执行失败时，此值为空，执行成功时，此值为执行结果

<a name="client-addmixstreams"></a>

### 48. addMixStreams 方法

可在录制或转推时，为录制或转推添加需要混合的流，示例代码：

```
client.addMixStreams(AddMixStreamsOptions, callback)
```

#### 参数说明

- AddMixStreamsOptions: object 类型, 必传，详细的类型说明如下

```
{
  streams: MixStream[]  // 必传，指定需要混合的新流
}
```
> 点击 [MixStream](#mixstream) 见详细的类型说明


- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, Result) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

Result 为返回值，[MixResult 类型](#mixresult)，执行失败时，此值为空，执行成功时，此值为执行结果


<a name="client-removemixstreams"></a>

### 49. removeMixStreams 方法

可在录制或转推时，删除录制或转推中混合的流，示例代码：

```
client.removeMixStreams(RemoveMixStreamsOptions, callback)
```

#### 参数说明

- RemoveMixStreamsOptions: object 类型, 必传，详细的类型说明如下

```
{
  streams: MixStream[]  // 必传，指定需要混合的新流
}
```
> 点击 [MixStream](#mixstream) 见详细的类型说明

- callback: function 类型，选传，方法的回调函数，函数说明如下

```
function callback(Err, Result) {}
```
Err 为返回值，为空时，说明已执行成功，否则执行失败，值为执行失败的错误信息

Result 为返回值，[MixResult 类型](#mixresult)，执行失败时，此值为空，执行成功时，此值为执行结果

----

<a name='getdevices'></a>

## 二、getDevices 方法

用于获取当前浏览器可访问的音视频设备的设备信息，包括麦克风、摄像头、音频输出设备

> 注：若站点未经过用户授权浏览器使用麦克风、摄像头、音频输出设备，会弹出提示要求用户进行授权

```
UCloudRTC.getDevices(onSuccess, onFailure)
```

#### 参数说明

- onSuccess: 必传，函数类型，方法调用成功时执行的回调函数。

```
function(MediaDeviceInfos) {}
```

函数参数 MediaDeviceInfos 为返回值，MediaDeviceInfo 类型的数组，为一组输入、输出设备的描述信息，点击
[MediaDeviceInfo](https://developer.mozilla.org/en-US/docs/Web/API/MediaDeviceInfo) 查看其详情。

- onFailure: 选传，函数类型，方法调用失败时执行的回调函数。

```
function(Err) {}
```
Err 为错误信息

----

<a name='getsupportprofilenames'></a>

## 三、getSupportProfileNames 方法

用于获取当前 SDK 支持的视频质量的名称

```
const profileNames = UCloudRTC.getSupportProfileNames();
```

#### 返回值说明

- profileNames: String 类型的数组，如当前可用的 ["240\*180", "320\*180", "320\*240", "480\*360", "640\*360", "640\*480", "1280\*720", "1920\*1080"]

名称 | 视频宽高 | 帧率 | 视频最小带宽 | 视频最大带宽
:-: | :-: | :-: | :-: | :-:
"240\*180" | 240\*180 | 20 | 100 | 200
"320\*180" | 320\*180 | 20 | 100 | 300
"320\*240" | 320\*240 | 20 | 100 | 400
"480\*360" | 480\*360 | 20 | 100 | 400
"640\*360" | 640\*360 | 20 | 100 | 500
"640\*480" | 640\*480 | 20 | 100 | 600
"1280\*720" | 1280\*720 | 20 | 100 | 1000
"1920\*1080" | 1920\*1080 | 20 | 100 | 1500

---

<a name="getsupportedcodec"></a>

## 四. getSupportedCodec 方法

检测当前浏览器支持的音视频的编解码格式

```
UCloudRTC.getSupportedCodec(callback);
```

#### 参数说明

- callback: function 类型，必传，方法的回调函数，函数说明如下

```
function callback(Codecs) { }
```

Codecs 为返回值，object 类型，详细的类型说明如下

```
{
  audio: string[],  // 字符串数组，支持的音频编解码格式。可能含有 "opus"，或为空数组。
  video: string[],  // 字符串数组，支持的视频编解码格式。可能含有 "h264"、"vp8" 两种取值，或为空数组。
}
```

---

<a name="issupportwebrtc"></a>

## 五. isSupportWebRTC 方法

检测当前浏览器对 WebRTC 的适配情况

```
const result = UCloudRTC.isSupportWebRTC();
```

#### 返回值说明

- result: boolean 类型，
  - true: 当前使用的浏览器支持 WebRTC。
  - false: 当前使用的浏览器不支持 WebRTC。

---

<a name="devicedetection"></a>

### 六. deviceDetection 方法

发布本地流或启动预览时，有可能因为麦克风或摄像头设备问题（如驱动问题，或未经授权等），导致无法正常发布或预览。此方法可用于发布或预览前的设备检测，根据检测结果，再自行决定在发布或预览时启用麦克风或摄像头或麦克风和摄像头，示例代码：

```
UCloudRTC.deviceDetection(DeviceDetectionOptions, callback);
```

#### 参数说明

- DeviceDetectionOptions: object 类型, 必传，详细的类型说明如下

```
{
  audio: boolean          // 必填，指定是否检测麦克风设备
  video: boolean          // 必填，指定是否检测摄像头设备
  microphoneId?: string   // 选填，指定需要检测的麦克风设备的ID，可通过 getMicrophones 方法查询获得该ID，不填时，将检测默认的麦克风设备
  cameraId?: string       // 选填，指定需要检测的摄像头设备的ID，可以通过 getCameras 方法查询获得该ID，不填时，将检测默认的摄像头设备
}
```

- callback: function 类型，必传，方法的回调函数，函数说明如下

```
function callback(Result) {
  if (Result.audio && Result.video) {
    // 麦克风和摄像头都可有和，发布或预览时可启用麦克风和摄像头
    // client.publish({audio: true, video: true});
  } else if (Result.audio) {
    // 麦克风可用，发布或预览时能启用麦克风
    // client.publish({audio: true, video: false});
  } else if (Result.video) {
    // 摄像头可用，发布或预览时能启用摄像头
    // client.publish({audio: false, video: true});
  } else {
    // 麦克风和摄像头都不可用
  }
}
```

Result 为返回值，DeviceDetectionResult 类型，详细的类型说明如下

```
{
  audio: boolean,       // 指麦克风设备是否可用
  audioError?: string   // 当麦克风不可用时，此字段可说明设备不可用的原因
  video: boolean        // 指摄像头设备是否可用
  videoError?: string   // 当摄像头不可用时，此字段可说明设备不可用的原因
}
```

> audioError 和 videoError 可能的错误信息有：[参见](#device-error)


---

<a name='version'></a>

## 七、version 属性

version 属性用于显示当前 sdk 的版本

---

<a name='generateToken'></a>

## 八、generateToken 方法

generateToken 方法仅用于试用 URTC 产品时替代服务器生成 sdk 所需 token 的方法，正式使用 URTC 产品时，需要搭建后台服务按规则生成 token

```
const token = UCloudRTC.generateToken(AppId, AppKey, RoomId, UserId);
```

#### 参数说明

- AppId: string 类型, 必传，可从 UCloud 控制台查看

- AppKey: string 类型, 必传，可从 UCloud 控制台查看（请注意此 AppKey 不可暴露给其他人）

- RoomId: string 类型, 必传，将要加入的房间的 ID

- UserId: string 类型，必传，将要加入的用户的 ID

---

<a name='logger'></a>

## 九、Logger 对象

Logger 对象用于调试时打印内部日志，包含以下方法：

* [setLogLevel 方法](#logger-setloglevel)
* [debug 方法](#logger-debug)
* [info 方法](#logger-info)
* [warn 方法](#logger-warn)
* [error 方法](#logger-error)

<a name='logger-setloglevel'></a>

### 1. setLogLevel 方法

用于设置 Logger 打印日志的级别

```
Logger.setLogLevel(Level)
```

#### 参数说明

Level: 必传，有 "debug" | "info" | "warn" | "error" 四个日志级别，默认为 "warn" 级别

<a name='logger-debug'></a>

### 2. debug 方法

用于调试代码时，打印 debug 日志

```
Logger.debug(a, ...)  // 可传任意数量的任意类型的变量作为参数
```

<a name='logger-info'></a>

### 3. info 方法

<a name='logger-warn'></a>

### 4. warn 方法

<a name='logger-error'></a>

### 5. error 方法

以上三种方法分别打印对应级别的日志，使用方法与 debug 方法相同

---

<a name='setservers'></a>

## 十、setServers 方法

可配置 URTC 服务的域名，用于私有化部署，目前有房间服务器和日志服务器的两种域名可进行配置，示例代码：

```
UCloudRTC.setServers({
  api: "https://env1.urtc.com",   // api 为 URTC 房间服务的访问地址
  log: "https://env1.urtclog.com" // log 为 URTC 日志服务的访问地址
  signal: "wss://env1.urtcsignal.com.cn:5005" // signal 为 URTC 信令服务的访问地址
})
```

> 注：
> 
> 1. 当不需要日志服务（未部署日志服务器）时，可不用设置 log。
> 2. 当仅部署一台信令服务器时，由于可直接指定信令服务的访问地址，不需要由房间服务分配，此时可仅设置 signal，不用设置 api，如：`UCloudRTC.setServers({signal: "wss://env1.urtcsignal.com.cn:5005"})`。
> 3. 当设置了 signal 时，优先使用 signal 指定的信令服务的访问地址，不再调用由 api 指定的房间服务分配信令服务访问地址的接口。


# ** Windows **

URTC Windows SDK包含以下方法：

* [一、UcloudRtcEngine引擎接口 类](#class)
    * [1.1  获取引擎 - UCloudRtcEngine](#class-UCloudRtcEngine)
    * [1.2  绑定监听事件 - regRtcEventListener ](#class-regRtcEventListener)
    * [1.3  设置SDK模式 - setSdkMode](#class-setSdkMode)
    * [1.4  设置通话模式 - setChannelType](#class-setChannelType)	
    * [1.5  设置流操作权限 - setStreamRole](#class-setStreamRole)
    * [1.6  设置纯音频模式 - setAudioOnlyMode](#class-setAudioOnlyMode)	
    * [1.7  设置自动发布和订阅 - setAutoPublishSubscribe](#class-setAutoPublishSubscribe)
    * [1.8  配置本地音频是否发布 - configLocalAudioPublish](#class-configLocalAudioPublish)
    * [1.9  是否发布音频 - isLocalAudioPublishEnabled](#class-isLocalAudioPublishEnabled)
    * [1.10  配置是否自动发布本地摄像头 - configLocalCameraPublish](#class-configLocalCameraPublish)
    * [1.11  是否发布摄像头 - isLocalCameraPublishEnabled](#class-isLocalCameraPublishEnabled)
    * [1.12  配置是否自动发布本地桌面 - configLocalScreenPublish](#class-configLocalScreenPublish)
    * [1.13  是否发布桌面 - isLocalScreenPublishEnabled](#class-isLocalScreenPublishEnabled)
    * [1.14  设置视频编码参数 - setVideoProfile](#class-setVideoProfile)
    * [1.15  入会时关闭摄像头 - muteCamBeforeJoin](#class-muteCamBeforeJoin)	
    * [1.16  入会时关闭麦克风 - muteMicBeforeJoin](#class-muteMicBeforeJoin)		
    * [1.17  加入房间 - joinChannel](#class-joinChannel)
    * [1.18  离开房间 - leaveChannel](#class-leaveChannel)	
    * [1.19  发布本地流 - publish](#class-publish)
    * [1.20  停止发布本地流 - unPublish](#class-unPublish)
    * [1.21  开启本地渲染 - startPreview](#class-startPreview)	
    * [1.22  停止本地渲染 - stopPreview](#class-stopPreview)	
    * [1.23 订阅远端媒体流 - subscribe](#class-subscribe)
    * [1.24 取消订阅远端媒体流 - unSubscribe](#class-unSubscribe)
    * [1.25  开启远端渲染 - startRemoteView](#class-startRemoteView)	
    * [1.26  停止远端渲染 - stopRemoteView](#class-stopRemoteView)	
    * [1.27  打开/关闭本地麦克风 - muteLocalMic](#class-muteLocalMic)	
    * [1.28  打开/关闭本地视频 - muteLocalVideo](#class-muteLocalVideo)		
    * [1.29  应用静音 - enableAllAudioPlay](#class-enableAllAudioPlay)	
    * [1.30  打开/关闭远端音频 - muteRemoteAudio](#class-muteRemoteAudio)	
    * [1.31  打开/关闭远端视频 - muteRemoteVideo](#class-muteRemoteVideo)	
    * [1.32  是否为自动发布模式 - isAutoPublish](#class-isAutoPublish)	
    * [1.33  是否为自动订阅模式 - isAutoSubscribe](#class-isAutoSubscribe)	
    * [1.34  切换摄像头 - switchCamera](#class-switchCamera)
    * [1.35  设置RTSP视频源 - enableExtendRtspVideocapture](#class-enableExtendRtspVideocapture)
    * [1.36  设置自定义外部视频源 - enableExtendVideocapture](#class-enableExtendVideocapture)	
    * [1.37  设置音频数据监听 - regAudioFrameCallback](#class-regAudioFrameCallback)	
    * [1.38  添加micphone混音文件 - startAudioMixing](#class-startAudioMixing)	
    * [1.39  停止micphone混音 - stopAudioMixing](#class-stopAudioMixing)	
    * [1.40  设置桌面编码参数 - setDesktopProfile](#class-setDesktopProfile)	
    * [1.41  设置桌面采集参数 - setCaptureScreenPagrams](#class-setCaptureScreenPagrams)	
    * [1.42  设置桌面采集类型 - setUseDesktopCapture](#class-setUseDesktopCapture)	
    * [1.43  获取屏幕个数 - getDesktopNums](#class-getDesktopNums)	
    * [1.44  获取屏幕信息 - getDesktopInfo](#class-getDesktopInfo)	
    * [1.45  获取窗口个数 - getWindowNums](#class-getWindowNums)
    * [1.46  获取窗口信息 - getWindowInfo](#class-getWindowInfo)	
    * [1.48  开启录制 - startRecord](#class-startRecord)	
    * [1.49  停止录制 - stopRecord](#class-stopRecord)	
    * [1.50  设置日志等级 - setLogLevel](#class-setLogLevel)	
    * [1.51  获取SDK 版本 - getSdkVersion](#class-getSdkVersion)	
    * [1.52  销毁引擎 - destroy](#class-destroy)
	* [1.53  设置外部音频采集 - enableExtendAudiocapture](#class-enableExtendAudiocapture)
	* [1.54  注册设备热插拔回调通知 - regDeviceChangeCallback](#class-regDeviceChangeCallback)
	* [1.55  旁路推流 - addPublishStreamUrl](#class-addPublishStreamUrl)
	* [1.56  停止旁路推流 - removePublishStreamUrl](#class-removePublishStreamUrl)
	* [1.57  更新旁路推流合流的流 - updateRtmpMixStream](#class-updateRtmpMixStream)
* [二、UcloudMediaDevice设备引擎接口类](#Device)    
    * [2.1  初始化设备模块 - UCloudRtcMediaDevice](#Device-UCloudRtcMediaDevice)		
    * [2.2  销毁设备模块 - destory](#Device-destory)			
    * [2.3  初始化视频模块 - InitVideoMoudle](#Device-InitVideoMoudle)		
    * [2.4  销毁视频模块 - UnInitVideoMoudle](#Device-UnInitVideoMoudle)		
    * [2.5  初始化音频模块 - InitAudioMoudle](#Device-InitAudioMoudle)		
    * [2.6  销毁音频模块 - UnInitAudioMoudle](#Device-UnInitAudioMoudle)			
    * [2.7  获取摄像头数量 - getCamNums](#Device-getCamNums)		
    * [2.8  获取麦克风数量 - getRecordDevNums](#Device-getRecordDevNums)			
    * [2.9  获取播放设备数量 - getPlayoutDevNums](#Device-getPlayoutDevNums)		
    * [2.10  获取摄像头设备信息 - getVideoDevInfo](#Device-getVideoDevInfo)			
    * [2.11  获取麦克风设备信息 - getRecordDevInfo](#Device-getRecordDevInfo)		
    * [2.12  获取播放设备信息 - getPlayoutDevInfo](#Device-getPlayoutDevInfo)		
    * [2.13  获取当前使用的摄像头信息 - getCurCamDev](#Device-getCurCamDev)		
    * [2.14  获取当前使用的麦克风设备信息 - getCurRecordDev](#Device-getCurRecordDev)		
    * [2.15  获取当前使用的播放设备信息 - getCurPlayoutDev](#Device-getCurPlayoutDev)		
    * [2.16  设置视频设备 - setVideoDevice](#Device-setVideoDevice)		
    * [2.17  设置麦克风设备 - setRecordDevice](#Device-setRecordDevice)			
    * [2.18  设置播放设备 - setPlayoutDevice](#Device-setPlayoutDevice)		
    * [2.19  获取应用播放音量 - getPlaybackDeviceVolume](#Device-getPlaybackDeviceVolume)			
    * [2.20  设置应用播放音量 - setPlaybackDeviceVolume](#Device-setPlaybackDeviceVolume)		
    * [2.21  获取系统麦克风音量 - getRecordingDeviceVolume](#Device-getRecordingDeviceVolume)			
    * [2.22  设置系统麦克风音量 - setRecordingDeviceVolume](#Device-setRecordingDeviceVolume)		
    * [2.23  开始摄像头测试 - startCamTest](#Device-startCamTest)		
    * [2.24  停止摄像头测试 - stopCamTest](#Device-stopCamTest)			
    * [2.25  开始麦克风测试 - startRecordingDeviceTest](#Device-startRecordingDeviceTest)		
    * [2.26  停止麦克风测试 - stopRecordingDeviceTest](#Device-stopRecordingDeviceTest)		
    * [2.27  开始播放设备测试 - startPlaybackDeviceTest](#Device-startPlaybackDeviceTest)		
    * [2.28  停止播放设备测试 - stopPlaybackDeviceTest](#Device-stopPlaybackDeviceTest)			
    * [2.29  开始采集视频数据回调 - startCaptureFrame](#Device-startCaptureFrame)		
    * [2.30  停止采集视频数据回调 - stopCaptureFrame](#Device-stopCaptureFrame)			
* [三、接口错误表](#ErrCode)    
    * [3.1  事件回调错误码](#ErrCode-shijian)		
    * [3.2  函数值错误码](#ErrCode-hanshu)			
* [四、函数结构体说明](#struct)    
    * [4.1  设备信息类 - tUCloudRtcDeviceInfo](#struct-tUCloudRtcDeviceInfo)		
    * [4.2  媒体发布配置类 - tUCloudRtcMediaConfig](#struct-tUCloudRtcMediaConfig)		
    * [4.3  媒体轨道类型类型描述 - eUCloudRtcTrackType](#struct-eUCloudRtcTrackType)		
    * [4.4  媒体流类型描述 - eUCloudRtcMeidaType](#struct-eUCloudRtcMeidaType)		
    * [4.5  流信息结构体 - tUCloudRtcStreamInfo](#struct-tUCloudRtcStreamInfo)		
    * [4.6  录制水印位置 - eUCloudRtcWaterMarkPos](#struct-eUCloudRtcWaterMarkPos)		
    * [4.7  水印类型 - eUCloudRtcWaterMarkType](#struct-eUCloudRtcWaterMarkType)		
    * [4.8  Mute操作结构体 - tUCloudRtcMuteSt](#struct-tUCloudRtcMuteSt)		
    * [4.9  录制输出等级 - eUCloudRtcRecordProfile](#struct-eUCloudRtcRecordProfile)		
    * [4.10  录制媒体类型 - eUCloudRtcRecordType](#struct-eUCloudRtcRecordType)			
    * [4.11  录制配置信息 - tUCloudRtcRecordConfig](#struct-tUCloudRtcRecordConfig)		
    * [4.12  渲染模式 - eUCloudRtcRenderMode](#struct-eUCloudRtcRenderMode)		
    * [4.13  日志级别 - eUCloudRtcLogLevel](#struct-eUCloudRtcLogLevel)		
    * [4.14  视频质量参数 - eUCloudRtcVideoProfile](#struct-eUCloudRtcVideoProfile)		
    * [4.15  桌面输出参数 - eUCloudRtcScreenProfile](#struct-eUCloudRtcScreenProfile)		
    * [4.16  桌面采集参数 - tUCloudRtcScreenPargram](#struct-tUCloudRtcScreenPargram)		
    * [4.17  桌面采集类型 - eUCloudRtcDesktopType](#struct-eUCloudRtcDesktopType)		
    * [4.18  桌面参数 - tUCloudRtcDeskTopInfo](#struct-tUCloudRtcDeskTopInfo)		
    * [4.19  通道类型 - eUCloudRtcUserStreamRoleCHANNEL](#struct-eUCloudRtcUserStreamRoleCHANNEL)		
    * [4.20  流权限 - eUCloudRtcUserStreamRoleSTREAM](#struct-eUCloudRtcUserStreamRoleSTREAM)	
    * [4.21  渲染窗口 - tUCloudRtcVideoCanvas](#struct-tUCloudRtcVideoCanvas)		
    * [4.22  登录信息类 - tUCloudRtcAuth](#struct-tUCloudRtcAuth)		
    * [4.23  当前媒体状态统计 - tUCloudRtcStreamStats](#struct-tUCloudRtcStreamStats)		
    * [4.24  录制信息回调 - tUCloudRtcRecordInfo](#struct-tUCloudRtcRecordInfo)		
    * [4.25  音频帧 - tUCloudRtcAudioFrame](#struct-tUCloudRtcAudioFrame)		
    * [4.26  视频数据帧类型 - eUCloudRtcVideoFrameType](#struct-eUCloudRtcVideoFrameType)		
    * [4.27  视频数据帧 - tUCloudRtcVideoFrame](#struct-tUCloudRtcVideoFrame)		
    * [4.28  消息回调事件接口类 - UCloudRtcEventListener](#struct-UCloudRtcEventListener)		
    * [4.29  音频测试回调 - UCloudRtcMediaListener](#struct-UCloudRtcMediaListener)		
    * [4.30  音频数据回调 - UCloudRtcAudioFrameCallback](#struct-UCloudRtcAudioFrameCallback)			
    * [4.31  视频扩展数据源 - UCloudRtcExtendVideoCaptureSource](#struct-UCloudRtcExtendVideoCaptureSource)		
    * [4.32  视频数据回调 - UCloudRtcExtendVideoRender](#struct-UCloudRtcExtendVideoRender)		
    * [4.33  视频数据回调监听类（yuv420p格式） - UCloudRtcVideoFrameObserver](#struct-UCloudRtcVideoFrameObserver)		
    * [4.34  视频渲染类型 - eUCloudRtcRenderType](#struct-eUCloudRtcRenderType)		
    * [4.35  视频编码类型 - eUCloudRtcVideoCodec](#struct-eUCloudRtcVideoCodec)		
    * [4.36  视频参数 - tUCloudVideoConfig](#struct-tUCloudVideoConfig)	
	* [4.37  网络上下类型 - eUCloudRtcNetworkQuality](#struct-eUCloudRtcNetworkQuality)
	* [4.38  网络评分 - eUCloudRtcQualityType](#struct-eUCloudRtcQualityType)
	* [4.39  热插拔回调 - UcloudRtcDeviceChanged](#struct-UcloudRtcDeviceChanged)	
    
	
<a name='class'></a>

## 一、 class UcloudRtcEngine引擎接口类

<a name='class-UCloudRtcEngine'></a>

### 1.1  获取引擎

static UCloudRtcEngine *sharedInstance()

**返回值**

UCloudRtcEngine* 引擎类指针

**参数说明**    

无

**消息回调**

无


### 1.2  绑定监听事件

void regRtcEventListener(UCloudRtcEventListener* listener)

**返回值** 

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  listener[in]   | UCloudRtcEventListener <br> eUCloudRtcSdkMode     | Class | N |

**消息回调**

无

### 1.3  设置SDK模式

virtual int setSdkMode (eUCloudRtcSdkMode mode)

**返回值**

参见错误描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  mode[in]   | 可以设置为测试模式、正式模式。 <br> 详见eUCloudRtcSdkMode。     | enum | N |

**消息回调**

无

<a name='class-setChannelType('></a>

###  1.4  设置应用模式

virtual int setChannelType(eUCloudRtcUserStreamRole TYPE)

可以设置为会议模式、直播模式。会议模式，默认都有推流权限；直播模式，推流、拉流权限，只能任选其一。

**返回值**

参见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  TYPE[in]   | 可以设置为会议模式、直播模式。 <br> 详见eUCloudRtcUserStreamRole说明。     | enum | N |



<a name='class-setStreamRole'></a>

###  1.5  设置流操作权限

virtual int setStreamRole(eUCloudRtcUserStreamRole role)

**返回值**

参见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  role[in]   | 可以设置为上行推流、下行拉流、双向推拉流。 <br> 详见UCloudStreamRole说明。     | enum | N |


**消息回调**

无

<a name='class-setAudioOnlyMode'></a>

###  1.6  设置纯音频模式

virtual int setAudioOnlyMode(bool audioOnly)

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  audioOnly[in]  | 是否设置为纯音频模式    | bool | N |

**消息回调**

无

<a name='class-setAutoPublishSubscribe'></a>

###  1.7  设置自动发布和订阅

virtual int setAutoPublishSubscribe(bool autoPub, bool autoSub)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  autoPub[in]  | 是否自动发布流     | bool | N |
|  autoSub[in]   | 是否自动订阅流     | bool | N |

**消息回调**

无


<a name='class-configLocalAudioPublish'></a>

###  1.8  配置是否自动发布本地音频

virtual int configLocalAudioPublish(bool enable)

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in]   | 配置自动发布本地音频可选：是、否     | bool | N |

**消息回调**

无


<a name='class-isLocalAudioPublishEnabled'></a>

###  1.9  是否发布音频

virtual bool isLocalAudioPublishEnabled()

是否发布音频，必须在配置自动发布本地音频时，才能调用。

**返回值**

true 发布音频;false 不发布音频。

**参数说明**    

无

**消息回调**

无


<a name='class-configLocalCameraPublish'></a>

###  1.10  配置是否自动发布本地摄像头

virtual int configLocalCameraPublish(bool enable)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in]   | 配置自动发布本地视频可选：是、否     | bool | N |

**消息回调**

无

<a name='class-isLocalCameraPublishEnabled'></a>

###  1.11  是否发布摄像头

virtual bool isLocalCameraPublishEnabled()

是否发布摄像头，必须在配置自动发布本地摄像头时，才能调用。

**返回值**

true 发布摄像头;false 不发布摄像头。

**参数说明**    

无

**消息回调**

无

<a name='class-configLocalScreenPublish'></a>

###  1.12  配置是否自动发布本地桌面

virtual int configLocalScreenPublish (bool enable)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in]   | 配置自动发布本地桌面可选：是、否     | bool | N |

**消息回调**

无

<a name='class-isLocalScreenPublishEnabled'></a>

###  1.13  是否发布桌面

virtual bool isLocalScreenPublishEnabled()

是否发布桌面，必须在配置自动发布本地桌面时，才能调用。

**返回值**

true 发布桌面;false 不发布桌面。

**参数说明**    

无

**消息回调**

无


<a name='class-setVideoProfile'></a>

###  1.14  设置视频编码参数

virtual void setVideoProfile(eUCloudRtcVideoProfile& profile，tUCloudVideoConfig& videoconfig)

设置视频编码参数，可选定义好的编码视频分辨率；也可以自定义编码参数，设置视频编码宽、高、帧率。


**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  profile[in]   | 设置视频编码分辨率，详见eUCloudRtcVideoProfile参数说明     | enum | N |
| videoconfig[in]    | 设置视频编码宽、高、帧率 | struct| N |

自定义编码参数时，profile必须为 UCLOUD_RTC_VIDEO_PROFILE_NONE，用户自定义videoconfig参数成员必须指定有效值。

profile为有效值时，后面的参数无意义。

**消息回调**

无

<a name='class-muteCamBeforeJoin'></a>

###  1.15  入会时关闭摄像头

virtual int muteCamBeforeJoin(bool mute)

入会时是否关闭摄像头。如果关闭摄像头，会发送黑屏帧。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  mute[in]   | true，关闭摄像头。 <br> false，打开摄像头。     | bool | N |

**消息回调**

无

<a name='class-muteMicBeforeJoin'></a>

###  1.16  入会时关闭麦克风

virtual int muteMicBeforeJoin (bool mute)

入会时是否关闭麦克风。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  mute[in]   | true，关闭麦克风。 <br> false，打开麦克风。     | bool | N |

**消息回调**

无

<a name='class-joinChannel'></a>

###  1.17  加入房间

virtual int joinChannel(tUCloudRtcAuth & auth)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  auth[in]   | 加入房间参数，包含AppId、RoomId、UserId、Token。<br>详见tUCloudRtcAuth说明。    | struct | N |

**消息回调**

无



<a name='class-leaveChannel'></a>

###  1.18  离开房间

virtual int leaveChannel()

**返回值**

详见错误码描述。

**参数说明**    

无

**消息回调**

code回调为0代表成功，其他代表失败。

具体参见 消息回调事件接口类对应接口virtual void onLeaveRoom(int code, const char* msg, const char* uid, const char* roomid) {}


<a name='class-publish'></a>

###  1.19  发布本地流

virtual int publish(eUCloudRtcMeidaTypetype, bool hasvideo, bool hasaudio)

发布本地流，支持发布摄像头、发布桌面。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 发布流类型可选:摄像头、桌面。<br> 详见eUCloudRtcMeidaType参数说明。    | enum | N |
|  hasvideo[in]  | 发布流是否带视频    | bool | N |
|  hasaudio[in] | 发布流是否带音频    | bool | N |

**消息回调**

0 成功；其他失败。

具体参见 消息回调事件接口类对应接口virtual void onLocalPublish (const int code, const char* msg, tUCloudRtcStreamInfo& info) {}

<a name='class-unPublish'></a>

###  1.20  停止发布本地流

virtual int unPublish(eUCloudRtcMeidaType type)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 发布流类型可选:摄像头、桌面。<br> 详见eUCloudRtcMeidaType参数说明。    | enum | N |

**消息回调**

0 成功；其他失败。

具体参见 消息回调事件接口类对应接口virtual void onLocalUnPublish (const int code, const char* msg, tUCloudRtcStreamInfo& info) {}

<a name='class-startPreview'></a>

###  1.21  开启本地渲染

virtual int startPreview(tUCloudRtcVideoCanvas& view)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  view[in]  | 包含UserId、StreamId、StreamMtype等。<br> 详见tUCloudRtcVideoCanvas参数说明。    | struct | N |

**消息回调**

无

<a name='class-stopPreview'></a>

###  1.22  停止本地渲染

virtual int stopPreview (tUCloudRtcVideoCanvas& view)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  view[in]  | 包含UserId、StreamId、StreamMtype等。<br> 详见tUCloudRtcVideoCanvas参数说明。    | struct | N |

**消息回调**

无

<a name='class-subscribe'></a>

###  1.23 订阅远端媒体流

virtual int subscribe(tUCloudRtcStreamInfo & info)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 包含UserId、StreamId等。<br> 详见eUCloudRtcMeidaType参数说明。    | enum | N |

**消息回调**

0 成功；其他失败。

具体参见 消息回调事件接口类对应接口virtual void onSubscribeResult(const int code, const char* msg, tUCloudRtcStreamInfo& info) {}

<a name='class-unSubscribe'></a>

###  1.24 取消订阅远端媒体流

virtual void unSubscribe(tUCloudRtcStreamInfo& info)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 包含UserId、StreamId等。<br> 详见eUCloudRtcMeidaType参数说明。    | enum | N |

**消息回调**

0 成功；其他失败。

具体参见 消息回调事件接口类对应接口virtual void onUnSubscribeResult(const int code, const char* msg, tUCloudRtcStreamInfo& info) {}

<a name='class-startRemoteView'></a>

###  1.25  开启远端渲染

virtual int startRemoteView (tUCloudRtcVideoCanvas & view)

开启远端渲染,必须在订阅成功后才能调用。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  view[in]  | 包含UserId、StreamId、StreamMtype等。<br> 详见tUCloudRtcVideoCanvas参数说明。    | struct | N |

**消息回调**

无

<a name='class-stopRemoteView'></a>

###  1.26  停止远端渲染

virtual int stopRemoteView (tUCloudRtcVideoCanvas & view)

停止远端渲染,必须在订阅成功后才能调用。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  view[in]  | 包含UserId、StreamId、StreamMtype等。<br> 详见tUCloudRtcVideoCanvas参数说明。    | struct | N |

**消息回调**

无


<a name='class-muteLocalMic'></a>

###  1.27  打开/关闭本地麦克风

virtual int muteLocalMic(bool mute)

是否关闭本地麦克风。关闭后发送静音包。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  mute[in]   | 是否关闭本地麦克风     | bool | N |

**消息回调**

0 成功；其他失败。

具体参见 消息回调事件接口类对应接口virtual void onLocalStreamMuteRsp(const int code, const char* msg, eUCloudRtcMeidaType mediatype, eUCloudRtcTrackType tracktype, bool mute) {}


<a name='class-muteLocalVideo'></a>

###  1.28  打开/关闭本地视频

virtual int muteLocalVideo(bool mute, eUCloudRtcMeidaType mtype)

是否关闭本地视频。关闭后，发送黑屏帧。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  mute[in]   | 是否关闭本地视频     | bool | N |
|  mtype[in]  | 流类型可选:摄像头、桌面。<br> 详见eUCloudRtcMeidaType参数说明。      | enum | N |

**消息回调**

无


<a name='class-enableAllAudioPlay'></a>

###  1.29  应用静音

virtual int enableAllAudioPlay(bool enable)

关闭应用的声音，静音。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in]  | true，打开声音播放。 <br> false，关闭应用声音。     | bool | N |

**消息回调**

无


<a name='class-muteRemoteAudio'></a>

###  1.30  打开/关闭远端音频

virtual int muteRemoteAudio (tUCloudRtcStreamInfo& info,bool mute)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 包含UserId、StreamId等。<br> 详见tUCloudRtcStreamInfo参数说明。    | enum | N |
|  mute[in]   | true，关闭远端音频。 <br> false，打开远端音频。     | bool | N |

**消息回调**

code回调为0代表成功，其他代表失败。

具体参见 消息回调事件接口类对应接口virtual void onRemoteStreamMuteRsp(const int code, const char* msg, const char* uid, eUCloudRtcMeidaType mediatype, eUCloudRtcTrackType tracktype, bool mute) {}


<a name='class-muteRemoteVideo'></a>

###  1.31  打开/关闭远端视频

virtual int muteRemoteVideo(tUCloudRtcStreamInfo& info, bool mute)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]   | 包含UserId、StreamId等。<br> 详见tUCloudRtcStreamInfo参数说明。    | enum | N |
|  mute[in]   | true，关闭远端视频。 <br> false，打开远端视频。     | bool | N |

**消息回调**

code回调为0代表成功，其他代表失败。

具体参见 消息回调事件接口类对应接口virtual void onRemoteStreamMuteRsp(const int code, const char* msg, const char* uid, eUCloudRtcMeidaType mediatype, eUCloudRtcTrackType tracktype, bool mute) {}


<a name='class-isAutoPublish'></a>

###  1.32  是否为自动发布模式

virtual bool isAutoPublish()

**返回值**

true 是，false 不是。

**参数说明**    

无

**消息回调**

无


<a name='class-isAutoSubscribe'></a>

###  1.33  是否为自动订阅模式

virtual bool isAutoSubscribe()

**返回值**

true 是，false 不是。

**参数说明**    

无

**消息回调**

无


<a name='class-switchCamera'></a>

###  1.34  切换摄像头

virtual int switchCamera(tUCloudRtcDeviceInfo& info)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  info[in]  | 切换的摄像头设备信息    | tUCloudRtcDeviceInfo| N |

**消息回调**

无

<a name='class-enableExtendRtspVideocapture'></a>

###  1.35  设置RTSP视频源

virtual int enableExtendRtspVideocapture(eUCloudRtcMeidaType type, bool enable, const char* rtspurl)

设置RTSP视频源，支持设置2路RTSP视频源。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  type[in]  | rtsp要替换的视频源     | eUCloudRtcMeidaType| N |
|  enable[in]  | 是否开启RTSP源输入     | bool | N |
|  rtspurl[in]  | rtsp 地址     | char* | N |

**消息回调**

无

<a name='class-enableExtendVideocapture'></a>

###  1.36  设置自定义外部视频源

virtual int enableExtendVideocapture(bool enable, UCloudRtcExtendVideoCaptureSource* videocapture)

设置自定义外部视频源。

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in] | 是否开启自定义外部视频源输入     | bool| N |
|  Videocapture[in]  | 自定义外部数据源     | UCloudRtcExtendVideoCaptureSource | N |

**消息回调**

无

<a name='class-regAudioFrameCallback'></a>

###  1.37  设置音频数据监听

virtual void regAudioFrameCallback(UCloudRtcAudioFrameCallback* callback)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  callback[in]  | 音频回调监听     | UCloudRtcAudioFrameCallback | N |

**消息回调**

无


<a name='class-startAudioMixing'></a>

###  1.38  添加micphone混音文件

virtual int startAudioMixing(const char* filepath, bool replace, bool loop,float musicvol)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  filepath [in]   | 文件地址     | const char*| N |
|  replace[in]     | 是否取代mic 输入:true 替代、false 混合     | bool| N |
|  loop[in]  | 是否循环播放：true 循环播放、false 一次播放    | bool| N |
|  musicvol[in]   | 背景音音量（0.0 – 1.0），1.0表示原始音量    | float| N |

**消息回调**

无

<a name='class-stopAudioMixing'></a>

###  1.39  停止micphone混音

virtual int stopAudioMixing()

**返回值**

详见错误码描述。

**参数说明**    

无

**消息回调**

无




<a name='class-setDesktopProfile'></a>

###  1.40  设置桌面编码参数

virtual void setDesktopProfile (eUCloudRtcVideoProfile& profile)

设置桌面编码参数，可以设定桌面或者某一窗口的编码分辨率。

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  profile[in]   | 设置视频编码分辨率，详见eUCloudRtcVideoProfile参数说明     | enum | N |

**消息回调**

无

<a name='class-setCaptureScreenPagrams'></a>

###  1.41  设置桌面采集参数

virtual void setCaptureScreenPagrams (tUCloudRtcScreenPargram& par)

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  par[in]   | 设置包含窗口或者桌面id标识、起始坐标、宽、高。<br> 详见tUCloudRtcScreenPargram参数说明。     | struct | N |

**消息回调**

无

<a name='class-setUseDesktopCapture'></a>

###  1.42  设置桌面采集类型

virtual int setUseDesktopCapture(eUCloudRtcDesktopType desktoptype)

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  desktoptype [in]  | 可选采集的是桌面还是窗口。<br> 详见eUCloudRtcDesktopType参数说明。     | enum | N |

**消息回调**

无

<a name='class-getDesktopNums'></a>

###  1.43  获取屏幕个数

virtual void getDesktopNums ()

**返回值**

屏幕个数。

**参数说明**    

无

**消息回调**

无

<a name='class-getDesktopInfo'></a>

###  1.44  获取屏幕信息

virtual int getDesktopInfo(int pos, tUCloudRtcDeskTopInfo& info)

**返回值**

0 成功；其他失败。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  pos[in]   | 数组下标，个数通过getDesktopNums 获取     | int | N |
| info[in]    | 包含桌面/窗口类型、id 标识、桌面标题。<br> 详见tUCloudRtcDeskTopInfo参数说明。  | struct| N |

**消息回调**

无

<a name='class-getWindowNums'></a>

###  1.45  获取窗口个数

virtual void getWindowNums ()

**返回值**

窗口个数。

**参数说明**    

无

**消息回调**

无

<a name='class-getWindowInfo'></a>

###  1.46  获取窗口信息

virtual int getWindowInfo (int pos, tUCloudRtcDeskTopInfo& info)

**返回值**

0 成功；其他失败。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  pos[in]   | 数组下标，个数通过getWindowNums 获取     | int | N |
| info[in]    | 包含桌面/窗口类型、id 标识、桌面标题。<br> 详见tUCloudRtcDeskTopInfo参数说明。  | struct| N |

**消息回调**

无


<a name='class-startRecord'></a>

###  1.48  开启录制

virtual int startRecord(tUCloudRtcRecordConfig& recordconfig)

**返回值**

详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  recordconfig [in]  | 录制参数，具体详见tUCloudRtcRecordConfig。     | struct | N |

**消息回调**

code回调为0代表成功，其他代表失败。

具体参见 消息回调事件接口类对应接口virtual void onStartRecord(const int code, const char* msg, tUCloudRtcRecordInfo& info) {} 



<a name='class-stopRecord'></a>

###  1.49  停止录制

virtual int stopRecord ()

**返回值**

详见错误码描述。

**参数说明**    

无

**消息回调**

code回调为0代表成功，其他代表失败。

具体参见 消息回调事件接口类对应接口virtual void onStopRecord(const int code, const char* msg, const char* recordid) {} 


<a name='class-setLogLevel'></a>

###  1.50  设置日志等级

virtual void setLogLevel(eUCloudRtcLogLevel level)

**返回值**

无

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  level[in]   | 可以设置为DEBUG、消息、告警、错误、无告警。 <br> 详见eUCloudRtcLogLevel说明。     | enum | N |

**消息回调**

无


<a name='class-getSdkVersion'></a>

### 1.51  获取SDK 版本

static const char *getSdkVersion()

**返回值**

SDK的版本号

**参数说明**    

无

**消息回调**

无

<a name='class-destroy'></a>

### 1.52  销毁引擎

static void destroy()

**返回值**

无

**参数说明**    

无

**消息回调**

无

<a name='class-enableExtendAudiocapture'></a>

### 1.53  开启外部音频采集

int enableExtendAudiocapture(bool enable, UCloudRtcExtendAudioCaptureSource* audiocapture)

**返回值**

0 代表成功

**参数说明**    


| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  enable[in]   | 设置true还是false     | bool | N |
| audiocapture[in]    | 外部音频源。<br> 详见UCloudRtcExtendAudioCaptureSource参数说明。  | struct| N |

**消息回调**

无

<a name='class-regDeviceChangeCallback'></a>

### 1.54  注册设备热插拔回调通知

virtual void regDeviceChangeCallback(UcloudRtcDeviceChanged* callback)

**返回值**

无
**参数说明**    


| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
| callback[in]    | 回调通知句柄<br> 详见UcloudRtcDeviceChanged参数说明。  | struct| N |

**消息回调**

无

<a name='class-addPublishStreamUrl'></a>

### 1.55  旁路推流

virtual int addPublishStreamUrl(const char* url, tUCloudRtcTranscodeConfig *config)

**返回值**

0 成功
**参数说明**    


| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
| url[in]    | cdn地址<br>   | string| N |
| config[in]    | 转推配置<br> 详见tUCloudRtcTranscodeConfig参数说明。  | struct| N |
**消息回调**

void onRtmpStreamingStateChanged(const int 	state, const char* url, int code)

<a name='class-removePublishStreamUrl'></a>

### 1.56  停止旁路推流

virtual int removePublishStreamUrl(const char* url)

**返回值**

0 成功
**参数说明**    


| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
| url[in]    | cdn地址<br>   | string| N |

**消息回调**

void onRtmpStreamingStateChanged(const int 	state, const char* url, int code)

<a name='class-updateRtmpMixStream'></a>

### 1.57  更新旁路推流合流的流

virtual int updateRtmpMixStream(eUCloudRtmpOpration cmd, tUCloudRtcRelayStream* streams,int length)

**返回值**

0 成功
**参数说明**    


| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
| cmd[in]    | 操作类型<br> 详见eUCloudRtmpOpration参数说明。  | int| N |
| streams[in]    | 流列表<br> 详见tUCloudRtcRelayStream参数说明。  | struct| N |
| length[in]    | 流长度<br>   | int| N |

**消息回调**

virtual void onRtmpUpdateMixStreamRes(eUCloudRtmpOpration& cmd,const int code, const char* msg)



<a name='Device'></a>

## 二、UcloudMediaDevice设备引擎接口类

<a name='Device-UCloudRtcMediaDevice'></a>

###  2.1  初始化设备模块

static UCloudRtcMediaDevice*sharedInstance()

**返回值**

UCloudRtcMediaDevice*  设备类指针

**参数说明**    

无

**消息回调**

无

<a name='Device-destory'></a>

###  2.2  销毁设备模块

static void destory()

**返回值**

无

**参数说明**    

无

**消息回调**

无

<a name='Device-InitVideoMoudle'></a>

###  2.3  初始化视频模块

virtual int InitVideoMoudle()

**返回值**

code回调为0代表成功，其他代表失败。

**参数说明**    

无

**消息回调**

无

<a name='Device-UnInitVideoMoudle'></a>

###  2.4  销毁视频模块

virtual int UnInitVideoMoudle()

**返回值**

0 成功，其他失败。

**参数说明**    

无

**消息回调**

无

<a name='Device-InitAudioMoudle'></a>

###  2.5  初始化音频模块

virtual int InitAudioMoudle ()

**返回值**

code回调为0代表成功，其他代表失败。

**参数说明**    

无

**消息回调**

无


<a name='Device-UnInitAudioMoudle'></a>

###  2.6  销毁音频模块

virtual int UnInitAudioMoudle()

**返回值**

code回调为0代表成功，其他代表失败。

**参数说明**    

无

**消息回调**

无

<a name='Device-getCamNums'></a>

###  2.7  获取摄像头数量

virtual int getCamNums()

**返回值**

摄像头的数量。

**参数说明**    

无

**消息回调**

无

<a name='Device-getRecordDevNums'></a>

###  2.8  获取麦克风数量

virtual int getRecordDevNums()

**返回值**

获取的麦克风数量。

**参数说明**    

无

**消息回调**

无

<a name='Device-getPlayoutDevNums'></a>

###  2.9  获取播放设备数量

virtual int getPlayoutDevNums()

**返回值**

获取的播放设备数量。

**参数说明**    

无

**消息回调**

无

<a name='Device-getVideoDevInfo'></a>

###  2.10  获取摄像头设备信息

virtual int getVideoDevInfo(int index, char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  index[in]  | 对应的数组下标     | int | N |
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无

<a name='Device-getRecordDevInfo'></a>

###  2.11  获取麦克风设备信息

virtual int getRecordDevInfo(int index, char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  index[in]  | 对应的数组下标     | int | N |
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无


<a name='Device-getPlayoutDevInfo'></a>

###  2.12  获取播放设备信息

virtual int getPlayoutDevInfo(int index, char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  index[in]  | 对应的数组下标     | int | N |
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无

<a name='Device-getCurCamDev'></a>

###  2.13  获取当前使用的摄像头信息

virtual int getCurCamDev (char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |


**消息回调**

无


<a name='Device-getCurRecordDev'></a>

###  2.14  获取当前使用的麦克风设备信息

virtual int getCurRecordDev (char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无


<a name='Device-getCurPlayoutDev'></a>

###  2.15  获取当前使用的播放设备信息

virtual int getCurPlayoutDev (char devname[MAX_DEVICE_NAME_LEN], char devid[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devname[in] | 设备名称     | char数组 | N |
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无

<a name='Device-setVideoDevice'></a>

###  2.16  设置视频设备

virtual int setVideoDevice(const char deviceId[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无


<a name='Device-setRecordDevice'></a>

###  2.17  设置麦克风设备

virtual int setRecordDevice(const char deviceId[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无

<a name='Device-setPlayoutDevice'></a>

###  2.18  设置播放设备

virtual int setPlayoutDevice (const char deviceId[MAX_DEVICE_NAME_LEN])

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  devid[in]  | 设备id 号     | char数组 | N |

**消息回调**

无

<a name='Device-getPlaybackDeviceVolume'></a>

###  2.19  获取应用播放音量

virtual int getPlaybackDeviceVolume (int *volume)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  volume[out] | 系统播放音量（0-255）     | int 指针 | N |

**消息回调**

无

<a name='Device-setPlaybackDeviceVolume'></a>

###  2.20  设置应用播放音量

virtual int setPlaybackDeviceVolume (int volume)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  volume[out] | 系统播放音量（0-255）     | int 指针 | N |

**消息回调**

无

<a name='Device-getRecordingDeviceVolume'></a>

###  2.21  获取系统麦克风音量

virtual int getRecordingDeviceVolume (int *volume)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  volume[out] | 系统麦克风音量（0-255）  | int 指针 | N |

**消息回调**

无

<a name='Device-setRecordingDeviceVolume'></a>

###  2.22  设置系统麦克风音量

virtual int setRecordingDeviceVolume (int volume)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  volume[out] | 系统麦克风音量（0-255）  | int 指针 | N |

**消息回调**

无

<a name='Device-startCamTest'></a>

###  2.23  开始摄像头测试

virtual int startCamTest(const char deviceId[MAX_DEVICE_NAME_LEN]，UCloudRtcVideoProfile& profile , void* videoview)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  deviceid[in]  | 摄像头的id     | char 数组 | N |
|  profile[in] | 摄像头的参数     | UCloudRtcVideoProfile | N |
|  videoview[in] | 显示的渲染窗口句柄    | char 数组 | N |

**消息回调**

无

<a name='Device-stopCamTest'></a>

###  2.24  停止摄像头测试

virtual int stopCamTest()

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

无

**消息回调**

无

<a name='Device-startRecordingDeviceTest'></a>

###  2.25  开始麦克风测试

virtual int startRecordingDeviceTest(UCloudRtcAudioLevelListener* audiolevel)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  volume[out] | 音频能量回调  | UCloudRtcAudioLevelListener| N |

**消息回调**

virtual void onMiceAudioLevel(int volume) {} volume 音频能量 （0--255）


<a name='Device-stopRecordingDeviceTest'></a>

###  2.26  停止麦克风测试

virtual int stopRecordingDeviceTest()

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

无

**消息回调**

无

<a name='Device-startPlaybackDeviceTest'></a>

###  2.27  开始播放设备测试

virtual int startPlaybackDeviceTest(const char* filePath)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  filePath[in]  | 播放文件的地址（wav 格式）     | const char* | N |

**消息回调**

无

<a name='Device-stopPlaybackDeviceTest'></a>

###  2.28  停止播放设备测试

virtual int stopPlaybackDeviceTest()

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

无

**消息回调**

无

<a name='Device-startCaptureFrame'></a>

###  2.29  开始采集视频数据回调

virtual int startCaptureFrame(eUCloudRtcVideoProfile profile,UCloudRtcVideoFrameObserver* observer)

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

| 名称    | 说明 | 数据类型 | 可空 |
| -| -| -| -|
|  profile[in]  | 采集的视频参数     | eUCloudRtcVideoProfile | N |
|  observer[in]  | 视频数据监听类     | UCloudRtcVideoFrameObserver | N |

**消息回调**

无

<a name='Device-stopCaptureFrame'></a>

###  2.30  停止采集视频数据回调

virtual int stopCaptureFrame()

**返回值**

code回调为0代表成功，其他代表失败。详见错误码描述。

**参数说明**    

无

**消息回调**

无

<a name='ErrCode'></a>

## 三、 接口错误表

<a name='ErrCode-shijian'></a>

###  3.1  事件回调错误码

```cpp
typedef enum _tUCloudRtcCallbackErrCode{    
	UCLOUD_RTC_CALLBACK_ERR_CODE_OK = 0,  //成功    
	UCLOUD_RTC_CALLBACK_ERR_SERVER_CON_FAIL = 5000, //服务器连接失败    
	UCLOUD_RTC_CALLBACK_ERR_SERVER_DIS, // 服务端连接断开    
	UCLOUD_RTC_CALLBACK_ERR_SAME_CMD,  //重复的调用    
	UCLOUD_RTC_CALLBACK_ERR_NOT_IN_ROOM, //未加入房间 无发进行操作     
	UCLOUD_RTC_CALLBACK_ERR_ROOM_JOINED, // 已加入房间 无需加入    
	UCLOUD_RTC_CALLBACK_ERR_SDK_INNER,   // SDK 内部错误    
	UCLOUD_RTC_CALLBACK_ERR_ROOM_RECONNECTING, // 重连中 请求无法投递          
	UCLOUD_RTC_CALLBACK_ERR_STREAM_PUBED,  // 流已经发布  无需发布    
	UCLOUD_RTC_CALLBACK_ERR_PUB_NO_DEV,  // 发布无可用 音频 视频设备    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_NOT_PUB, //流没有发布 无法对流进行操作    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_SUBED,  //流已经订阅  无需订阅    
	UCLOUD_RTC_CALLBACK_ERR_STREAM_NO_SUB, //流没有订阅  无法    
	UCLOUD_RTC_CALLBACK_ERR_SUB_NO_USER,   //无对应的用户 无法订阅    
	UCLOUD_RTC_CALLBACK_ERR_SUB_NO_STREAM,  // 无对应的媒体流    
	UCLOUD_RTC_CALLBACK_ERR_USER_LEAVING, // 用户正在离开房间  无法进行其他操作    
	UCLOUD_RTC_CALLBACK_ERR_NO_HAS_TRACK,  //无对应的媒体轨道    
	UCLOUD_RTC_CALLBACK_ERR_MSG_TIMEOUT, // 消息请求超时    
}tUCloudRtcCallbackErrCode;    
```

<a name='ErrCode-hanshu'></a>

###  3.2  函数值错误码

```cpp
typedef enum _tUCloudRtcReturnErrCode {    
	UCLOUD_RTC_RETURN_ERR_CODE_OK = 0,  //成功    
	UCLOUD_RTC_RETURN_ERR_AUTO_PUB = 1000, //自动发布    
	UCLOUD_RTC_RETURN_ERR_AUTO_SUB, //自动订阅    
	UCLOUD_RTC_RETURN_ERR_NOT_INIT, //引擎没有初始化    
	UCLOUD_RTC_RETURN_ERR_IN_ROOM, //已经加入房间    
	UCLOUD_RTC_RETURN_ERR_NOT_IN_ROOM, // 未加入房间    
	UCLOUD_RTC_RETURN_ERR_NOT_PUB_TRACK, //未发布对应媒体    
	UCLOUD_RTC_RETURN_ERR_INVAILED_PARGRAM, // 无效参数    
	UCLOUD_RTC_RETURN_ERR_INVAILED_WND_HANDLE, // 无效窗口句柄    
	UCLOUD_RTC_RETURN_ERR_INVAILED_MEDIA_TYPE, // 无效媒体类型    
	UCLOUD_RTC_RETURN_ERR_SUB_ONEMORE, // 最少订阅一种媒体    
	UCLOUD_RTC_RETURN_ERR_NO_PUB_ROLE, //无发布权限    
	UCLOUD_RTC_RETURN_ERR_NO_SUB_ROLE, //无订阅权限    
	UCLOUD_RTC_RETURN_ERR_CAM_NOT_ENABLE, //没有配置本地cam 发送    
	UCLOUD_RTC_RETURN_ERR_SCREEN_NOT_ENABLE, //没有配置本地screen 发送    
	UCLOUD_RTC_RETURN_ERR_AUDIO_MODE,        // 纯音频模式    
	UCLOUD_RTC_RETURN_ERR_SECKEY_INVALID ,    // seckey 无效    
	UCLOUD_RTC_RETURN_ERR_INVAILD_FILEPATH,    
	UCLOUD_RTC_RETURN_ERR_NOT_SUPORT_AUDIO_FORMAT,    
}tUCloudRtcReturnErrCode;    
```

<a name='struct'></a>

## 四、 函数结构体说明

<a name='struct-tUCloudRtcDeviceInfo'></a>

###  4.1  设备信息类

```cpp
typedef struct {    
	char mDeviceName[MAX_DEVICE_ID_LENGTH]; // 设备名称    
	char mDeviceId[MAX_DEVICE_ID_LENGTH];   // 设备id    
} tUCloudRtcDeviceInfo;    
```

<a name='struct-tUCloudRtcMediaConfig'></a>

###  4.2  媒体发布配置类

最少启用一种媒体
    
```cpp
typedef struct {    
	bool mVideoEnable; // 启用视频    
	bool mAudioEnable; // 启用音频    
}tUCloudRtcMediaConfig;    
```

<a name='struct-eUCloudRtcTrackType'></a>

###  4.3  媒体轨道类型类型描述

```cpp
typedef enum {    
	UCLOUD_RTC_TRACKTYPE_AUDIO = 1, // 音频轨道    
	UCLOUD_RTC_TRACKTYPE_VIDEO   // 视频轨道    
} eUCloudRtcTrackType;    
```
<a name='struct-eUCloudRtcMeidaType'></a>

###  4.4  媒体流类型描述

```cpp
typedef enum {    
   	UCLOUD_RTC_MEDIATYPE_NONE = 0, // 无效类型    
	UCLOUD_RTC_MEDIATYPE_VIDEO, // 摄像头    
	UCLOUD_RTC_MEDIATYPE_SCREEN  // 桌面流    
} eUCloudRtcMeidaType;    
```

<a name='struct-tUCloudRtcStreamInfo'></a>

###  4.5  流信息结构体

```cpp
typedef struct {    
	const char* mUserId;  // 用户id    
const char* mStreamId; // 流id    
	bool mEnableVideo;    //启用视频    
	bool mEnableAudio;    // 启用音频    
	bool mEnableData;     // 启用数据通道（暂时无效）    
	eUCloudRtcMeidaType mStreamMtype;// 流类型        
} tUCloudRtcStreamInfo;    
```

<a name='struct-eUCloudRtcWaterMarkPos'></a>


###  4.6  录制水印位置

```cpp
typedef enum {    
    UCLOUD_RTC_WATERMARKPOS_LEFTTOP = 1, //左上    
UCLOUD_RTC_WATERMARKPOS_LEFTBOTTOM， // 左下    
UCLOUD_RTC_WATERMARKPOS_RIGHTTOP， // 右上    
UCLOUD_RTC_WATERMARKPOS_LEFTBOTTOM， // 右下    
} eUCloudRtcWaterMarkPos;    
```

<a name='struct-eUCloudRtcWaterMarkType'></a>

###  4.7  水印类型

```cpp
typedef enum {    
	UCLOUD_RTC_WATERMARK_TYPE_TIME = 1, // 时间水印    
	UCLOUD_RTC_WATERMARK_TYPE_PIC, // 图片水印    
	UCLOUD_RTC_WATERMARK_TYPE_TEXT, // 文字水印    
} eUCloudRtcWaterMarkType;    
```

<a name='struct-tUCloudRtcMuteSt'></a>

###  4.8  Mute操作结构体

```cpp
typedef struct {        
	const char* mUserId; // 用户id    
    const char* mStreamId; // 流id    
	bool mMute;  //true 关闭媒体  false 打开媒体    
	eUCloudRtcMeidaType mStreamMtype; // 媒体流类型    
} tUCloudRtcMuteSt;    
```

<a name='struct-eUCloudRtcRecordProfile'></a>

###  4.9  录制输出等级

```cpp
typedef enum {
    UCLOUD_RTC_RECORDPROFILE_SD = 1, //标清 640*360
	UCLOUD_RTC_RECORDPROFILE_HD,    // 高清  1280*720
	UCLOUD_RTC_RECORDPROFILE_HDPLUS, //超清 1920*1080
} eUCloudRtcRecordProfile;
```

<a name='struct-eUCloudRtcRecordType'></a>

###  4.10  录制媒体类型

```cpp
typedef enum {
    UCLOUD_RTC_RECORDTYPE_AUDIOONLY = 1,  //直录音频（混音）
    UCLOUD_RTC_RECORDTYPE_AUDIOVIDEO     // 录制混音混流
} eUCloudRtcRecordType;
```

<a name='struct-tUCloudRtcRecordConfig'></a>

###  4.11  录制配置信息

```cpp
typedef struct UCloudRtcRecordConfig {
	const char* mMainviewuid;   //录制的主流用户id
	const char* mBucket;        //存储bucket
	const char* mBucketRegion;  //存储region
	eUCloudRtcRecordProfile mProfile;  //录制profile
	eUCloudRtcRecordType mRecordType;  //录制类型
	eUCloudRtcWaterMarkPos mWatermarkPos;  //水印位置
	eUCloudRtcMeidaType mMainviewmediatype;  //主流的媒体类型

	eUCloudRtcWaterMarkType mWaterMarkType;   //水印类型
	const char* mWatermarkUrl;		//水印url  为图片水印时
	bool mIsaverage;				//是否均分 (true .流式布局，false:讲课模式)
	int mMixerTemplateType;			//模板类型

	//新版录制新增参数
	tUCloudRtcRelayStream *mStreams; //混流的用户
	int mStreamslength; //混流的用户长度
	int mLayout; // 0.取决于mIsaverage 1.流式布局 2.讲课模式 3.自定义布局 4.模板自适应1 5.模板自适应2


	UCloudRtcRecordConfig() {
		mWatermarkUrl = nullptr;
		mMainviewuid = nullptr;
		mBucket = nullptr;
		mBucketRegion = nullptr;
		mStreams = nullptr;
		mLayout = MIX_LAYOUT_OLD;
	}
}tUCloudRtcRecordConfig;
```

<a name='struct-eUCloudRtcRenderMode'></a>

###  4.12  渲染模式

```cpp
typedef enum {
    UCLOUD_RTC_RENDER_MODE_DEFAULT = 0, //默认平铺
    UCLOUD_RTC_RENDER_MODE_FIT, //保持比例
    UCLOUD_RTC_RENDER_MODE_FILL   //平铺
} eUCloudRtcRenderMode;
```

<a name='struct-eUCloudRtcLogLevel'></a>

###  4.13  日志级别

```cpp
typedef enum {
	UCLOUD_RTC_LOG_LEVEL_DEBUG,
	UCLOUD_RTC_LOG_LEVEL_INFO,
	UCLOUD_RTC_LOG_LEVEL_WARN,
	UCLOUD_RTC_LOG_LEVEL_ERROR,
	UCLOUD_RTC_LOG_LEVEL_NONE,
} eUCloudRtcLogLevel;
```

<a name='struct-eUCloudRtcVideoProfile'></a>

###  4.14  视频质量参数

```cpp
typedef enum {
    UCLOUD_RTC_VIDEO_PROFILE_NONE = -1,
	UCLOUD_RTC_VIDEO_PROFILE_320_180 = 1,
	UCLOUD_RTC_VIDEO_PROFILE_320_240 = 2,
	UCLOUD_RTC_VIDEO_PROFILE_640_360 = 3,
	UCLOUD_RTC_VIDEO_PROFILE_640_480 = 4,
	UCLOUD_RTC_VIDEO_PROFILE_1280_720 = 5,
	UCLOUD_RTC_VIDEO_PROFILE_1920_1080 = 6，
    UCLOUD_RTC_VIDEO_PROFILE_1920_1080_PLUS = 7// 1920*2*1080
} eUCloudRtcVideoProfile;
```

<a name='struct-eUCloudRtcScreenProfile'></a>

###  4.15  桌面输出参数

```cpp
typedef enum {
    UCLOUD_RTC_SCREEN_PROFILE_LOW = 1, //640*360
	UCLOUD_RTC_SCREEN_PROFILE_MIDDLE, //960*540
	UCLOUD_RTC_SCREEN_PROFILE_HIGH，// 1280*720
    UCLOUD_RTC_SCREEN_PROFILE_HIGH_PLUS  // 1920*1080
} eUCloudRtcScreenProfile;
```

<a name='struct-tUCloudRtcScreenPargram'></a>

###  4.16  桌面采集参数

```cpp
typedef struct
{
	long mScreenId; // 窗或者桌面id标识
	int  mXpos; // 起始x坐标
	int mYpos; // 起始y坐标
	int mWidth;// 采集宽度
	int mHeight; // 采集高度
} tUCloudRtcScreenPargram;
```

<a name='struct-eUCloudRtcDesktopType'></a>

###  4.17  桌面采集类型

```cpp
typedef enum {
	UCLOUD_RTC_DESKTOPTYPE_SCREEN = 1, 采集桌面
	UCLOUD_RTC_DESKTOPTYPE_WINDOW 采集窗口
} eUCloudRtcDesktopType;
```

<a name='struct-tUCloudRtcDeskTopInfo'></a>

###  4.18  桌面参数

```cpp
typedef struct
{
	eUCloudRtcDesktopType mType;  桌面类型
	int mDesktopId;  id 标识
	char mDesktopTitle[MAX_WINDOWS_TILE_LEN]; //桌面标题
} tUCloudRtcDeskTopInfo;
```

<a name='struct-eUCloudRtcUserStreamRoleCHANNEL'></a>

###  4.19  通道类型

```cpp
typedef enum {
	UCLOUD_RTC_CHANNEL_TYPE_COMMUNICATION,   // 实时互动模式,
    UCLOUD_RTC_CHANNEL_TYPE_BROADCAST      // 直播模式
} eUCloudRtcUserStreamRole;
```

<a name='struct-eUCloudRtcUserStreamRoleSTREAM'></a>

###  4.20  流权限

```cpp
typedef enum {
	UCLOUD_RTC_USER_STREAM_ROLE_PUB, // 上行推流
	UCLOUD_RTC_USER_STREAM_ROLE_SUB, // 下行拉流
	UCLOUD_RTC_USER_STREAM_ROLE_BOTH //双向推拉流
} eUCloudRtcUserStreamRole;
```

<a name='struct-tUCloudRtcVideoCanvas'></a>

###  4.21  渲染窗口

```cpp
typedef struct 
{
    void* mVideoView;   // 渲染的窗口句柄
    const char* mUserId; // 用户id
    const char* mStreamId; //流id
	eUCloudRtcMeidaType mStreamMtype; // 媒体流类型
    eUCloudRtcRenderMode mRenderMode;  // 渲染模式
    eUCloudRtcRenderType mRenderType; // 自定义渲染 mVideoView 指定为自己的render 扩展类
} tUCloudRtcVideoCanvas;
```

<a name='struct-tUCloudRtcAuth'></a>

###  4.22  登录信息类

```cpp
typedef struct 
{
    const char* mAppId; // 平台分配的appid
	const char* mRoomId; // room 标识
	const char* mUserId; // 用户标识
	const char* mUserToken; // 用户通过用户服务器获取的token
} tUCloudRtcAuth;
```

<a name='struct-tUCloudRtcStreamStats'></a>

###  4.23  当前媒体状态统计

```cpp
typedef struct {
	const char* mUserId; // 用户id
    const char* mStreamId; //流id
	int mStreamMtype;// 媒体流类型 摄像头 桌面
	int mTracktype; // 媒体轨道类型 1 audio 2 video
	int mAudioBitrate = 0;     // audio bitrate,unit:bps
	int mVideoBitrate = 0;
	int mVideoWidth = 0;     // video width
	int mVideoHeight = 0;     // video height
	int mVideoFrameRate = 0;     // video frames per secon
	float mPacketLostRate = 0.0f;
} tUCloudRtcStreamStats;
```

<a name='struct-tUCloudRtcRecordInfo'></a>

###  4.24  录制信息回调

```cpp
typedef struct {
	const char* mRecordId;
	const char* mFileName;
	const char* mRegion;
	const char* mBucket;
	const char* mRoomid;
} tUCloudRtcRecordInfo;
```

<a name='struct-tUCloudRtcAudioFrame'></a>

###  4.25  音频帧

```cpp
typedef struct {
	const char* mUserId;
	const char* mStreamId;
	void* mAudioData; // 内存
	int mBytePerSimple; // 采样位数16bit
	int mSimpleRate;  // 采样频率
	int mChannels;    // 声道数
	int mNumSimples;  //采集样本数
} tUCloudRtcAudioFrame;
```

<a name='struct-eUCloudRtcVideoFrameType'></a>

###  4.26  视频数据帧类型

```cpp
typedef enum {
	UCLOUD_RTC_VIDEO_FRAME_TYPE_I420 = 1,
	UCLOUD_RTC_VIDEO_FRAME_TYPE_RGB24,
	UCLOUD_RTC_VIDEO_FRAME_TYPE_RGBA,
	UCLOUD_RTC_VIDEO_FRAME_TYPE_ARGB,
} eUCloudRtcVideoFrameType;
```

<a name='struct-tUCloudRtcVideoFrame'></a>

###  4.27  视频数据帧

```cpp
typedef struct {
	unsigned char* mDataBuf;
	int mWidth;
	int mHeight;
	eUCloudRtcVideoFrameType mVideoType;
} tUCloudRtcVideoFrame;
```

<a name='struct-UCloudRtcEventListener'></a>

###  4.28  消息回调事件接口类

下面所有code 为0 代表成功，其他代表失败。

```cpp
class  UCloudRtcEventListener
{
public：
// 服务器断开
virtual void onServerDisconnect() {}
// 加入房间通知
virtual void onJoinRoom(int code, const char* msg, const char* uid, const char* roomid) {}
// 离开房间通知
virtual void onLeaveRoom(int code, const char* msg, const char* uid, const char* roomid) {}
//房间重连中
virtual void onRejoining(const char* uid, const char* roomid) {}
// 房间重连成功
virtual void onReJoinRoom(const char* uid, const char* roomid) {}
// 本地流发布结果回调
virtual void onLocalPublish(const int code, const char* msg, tUCloudRtcStreamInfo & info) {}
// 本地流取消发布结果回调
virtual void onLocalUnPublish(const int code, const char* msg, tUCloudRtcStreamInfo & info) {}
// 远端有用户加入房间
virtual void onRemoteUserJoin(const char* uid) {}
// 远端有用户离开房间
virtual void onRemoteUserLeave(const char* uid,int reson) {}
// 远端用户发布视频
virtual void onRemotePublish(tUCloudRtcStreamInfo & info) {}
// 远端用户取消发布视频
virtual void onRemoteUnPublish(tUCloudRtcStreamInfo & info) {}
// 订阅某条视频流回调
virtual void onSubscribeResult(const int code, const char* msg, tUCloudRtcStreamInfo & info) {}
// 取消订阅某条视频流回调
virtual void onUnSubscribeResult(const int code, const char* msg, tUCloudRtcStreamInfo & info) {}
// 操作本地媒体流结果回调
virtual void onLocalStreamMuteRsp(const int code, const char* msg,	eUCloudRtcMeidaType mediatype, UCloudTracktype tracktype, bool mute) {}
// 操作远端媒体流结果回调
	virtual void onRemoteStreamMuteRsp(const int code, const char* msg, const char* uid, eUCloudRtcMeidaType mediatype, eUCloudRtcTrackType tracktype, bool mute) {}
// 远端媒体流状态回调
	virtual void onRemoteTrackNotify(const char* uid, eUCloudRtcMeidaType mediatype, eUCloudRtcTrackType tracktype, bool mute) {}
//发送状态信息
	virtual void onSendRTCStats(tUCloudRtcStreamStats & rtstats) {}
//接收状态信息
	virtual void onRemoteRTCStats(tUCloudRtcStreamStats rtstats) {}
//开启录制回调
virtual void onStartRecord (const int code, const char* msg, tUCloudRtcRecordInfo& info) {}
// 结束录制回调
virtual void onStopRecord (const int code, const char* msg, const char* recordid) {}
// 麦克风能量回调
	virtual void onMiceAudioLevel(int volume) {} 
// 远端能量回调
	virtual void onRemoteAudioLevel(const char* uid, int volume) {}
// 踢下线
virtual void onKickoff(int code) {}
// 警告
virtual void onWarning(int warn) {}
// 错误
    virtual void onError(int error) {}
//网络质量评分回调
	virtual void onNetworkQuality(const char* uid, eUCloudRtcNetworkQuality&rtype, eUCloudRtcQualityType& Quality) {}
//旁推状态回调
	virtual void onRtmpStreamingStateChanged(const int 	state, const char* url, int code) {};
//旁推更新混合流回调
	virtual void onRtmpUpdateMixStreamRes(eUCloudRtmpOpration& cmd,const int code, const char* msg) {};
};
```

<a name='struct-UCloudRtcMediaListener'></a>

###  4.29  音频测试回调

```cpp
class  UCloudRtcMediaListener
{
public:
	// 音频能量回调   （0--255）
    virtual void onMiceAudioLevel(int volume) {} 
};
```

<a name='struct-UCloudRtcAudioFrameCallback'></a>

###  4.30  音频数据回调

```cpp
class  UCloudRtcAudioFrameCallback
{
public:
	//本地音频回调
	virtual void onLocalAudioFrame(tUCloudRtcAudioFrame* audioframe) {}
   // 远端混音数据
	virtual void onRemoteMixAudioFrame(tUCloudRtcAudioFrame* audioframe) {} 
};
```

<a name='struct-UCloudRtcExtendVideoCaptureSource'></a>

###  4.31  视频扩展数据源

用户可以扩展自己的视频输入 音频通过doCaptureFrame 采集数据。

```cpp
class  UCloudRtcExtendVideoCaptureSource
{
public:
	virtual  bool doCaptureFrame(tUCloudRtcVideoFrame* videoframe) = 0; 
};
```

<a name='struct-UCloudRtcExtendVideoRender'></a>

###  4.32  视频数据回调

用户结合渲染 可以获取数据进行自己渲染。

```cpp
class _EXPORT_API UCloudRtcExtendVideoRender
{
public:
	virtual  void onRemoteFrame(const tUCloudRtcVideoFrame* videoframe) = 0;
};
```

<a name='struct-UCloudRtcVideoFrameObserver'></a>

###  4.33  视频数据回调监听类（yuv420p格式）

引擎内存回调camera 采集数据 和 扩展数据源使用方便做视频前置处理。

```cpp
class  UCloudRtcVideoFrameObserver
{
public:
	virtual  void onCaptureFrame(unsigned char* videoframe, int buflen) = 0;
};
```

<a name='struct-eUCloudRtcRenderType'></a>

###  4.34  视频渲染类型

```cpp
typedef enum {
    UCLOUD_RTC_RENDER_TYPE_GDI = 1,
	UCLOUD_RTC_RENDER_TYPE_D3D = 2，
    UCLOUD_RTC_RENDER_TYPE_EXTEND = 3
} eUCloudRtcRenderType;
```

<a name='struct-eUCloudRtcVideoCodec'></a>

###  4.35  视频编码类型

```cpp
typedef enum {
	UCLOUD_RTC_CODEC_VP8 = 1, //default
	UCLOUD_RTC_CODEC_H264
} eUCloudRtcVideoCodec;
```

<a name='struct-tUCloudVideoConfig'></a>

###  4.36  视频参数

```cpp
typedef struct {
	int mWidth; // 宽
	int mHeight; // 高
	int mFrameRate; // 帧率
} tUCloudVideoConfig;
```

<a name='eUCloudRtcNetworkQuality'></a>

###  4.37  上下行网络类型

```cpp
typedef enum {
	UCLOUD_RTC_NETWORK_TX = 1,  //上行
	UCLOUD_RTC_NETWORK_RX = 2,  //下行
}eUCloudRtcNetworkQuality;
```

<a name='struct-eUCloudRtcQualityType'></a>

###  4.38  网络质量评分

```cpp
typedef enum {
	UCLOUD_RTC_QUALITY_UNKNOWN = 0, //未知
	UCLOUD_RTC_QUALITY_DOWN = 1,  //很坏
	UCLOUD_RTC_QUALITY_BAD = 2,  //勉强能沟通但不顺畅
	UCLOUD_RTC_QUALITY_POOR =  3, //用户主观感受有瑕疵但不影响沟通
	UCLOUD_RTC_QUALITY_GOOD = 4, // 用户主观感觉和 excellent 差不多
	UCLOUD_RTC_QUALITY_EXCELLENT = 5, //网络质量极好
}eUCloudRtcQualityType;
```

<a name='struct-UcloudRtcDeviceChanged'></a>

###  4.39  设备热插拔回调

```cpp
class _EXPORT_API UcloudRtcDeviceChanged
{
public:
	virtual void onInterfaceArrival(const char* dccname, int len) {}  //设备插入
	virtual void onInterfaceRemoved(const char* dccname, int len) {}  //设备移除
	virtual void onInterfaceChangeValue(const char* dccname, int len) {} //设备属性改变
	virtual ~UcloudRtcDeviceChanged() {}
};
```


<a name='struct-UCloudRtcExtendAudioCaptureSource'></a>

###  4.40  音频外部采集

```cpp
class  _EXPORT_API UCloudRtcExtendAudioCaptureSource
{
public:
	virtual ~UCloudRtcExtendAudioCaptureSource() {}
	virtual  bool doCaptureAudioFrame(tUCloudRtcAudioFrame* audioframe) = 0;
};
```

<a name='struct-tUCloudRtcRelayStream'></a>

###  4.41  转推的流

```cpp
typedef struct UCloudRtcRelayStream {
	const char* mUid;          //用户id
	eUCloudRtcMeidaType mType; //媒体类型 1摄像头 2桌面
	UCloudRtcRelayStream() {
		mUid = nullptr;
		mType = UCLOUD_RTC_MEDIATYPE_NONE;
	}
}tUCloudRtcRelayStream;
```

<a name='struct-eUCloudMixLayout'></a>

###  4.42  转推混流操作类型

```cpp
typedef enum {
	MIX_LAYOUT_OLD,      //兼容之前模板
	MIX_LAYOUT_FLOW,	 //流式布局
	MIX_LAYOUT_TEACH,			 //讲课布局
	MIX_LAYOUT_CUSTOM,    //自定义
	MIX_LAYOUT_ADAPTION1, //自适应模板1
	MIX_LAYOUT_ADAPTION2, //自适应模板2
}eUCloudMixLayout;
```

<a name='struct-UCloudRtcTranscodeConfig'></a>

###  4.43  转推配置

```cpp
typedef struct UCloudRtcTranscodeConfig {
	tUCloudBackgroundColor mbgColor;  //背景色
	int mFramerate; //帧率
	int mBitrate;   //码率
	const char*  mMainViewUid; //主讲人的uid
	int mMainviewType; //主讲人放置的流类型
	int mWidth;  //输出分辨率宽度
	int mHeight; //输出分辨率高度
	eUCloudMixLayout mLayout; // 1.流式布局 2.讲课模式 3.自定义布局 4.模板自适应1 5.模板自适应2
	const char*  mStyle; //mLayout=3 时自定义风格内容
	int mLenth;
	tUCloudRtcRelayStream *mStreams; //混流的用户
	int mStreamslength; //长度
	UCloudRtcTranscodeConfig()
	{
		mLayout = MIX_LAYOUT_TEACH;
		mMainViewUid = nullptr;
		mStreams = nullptr;
		mStyle = nullptr;
		mStreamslength = 0;
	}
}tUCloudRtcTranscodeConfig;
```


# ** Android **

URTC Android SDK API文档可以[在这里查看](https://github.com/ucloud/urtc-android-demo)

# ** iOS **

URTC iOS SDK API文档可以[在这里查看](https://github.com/ucloud/urtc-ios-demo)

<!-- tabs:end -->

