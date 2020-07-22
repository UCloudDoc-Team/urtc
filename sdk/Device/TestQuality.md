# 通话前检测网络质量

通话前，检测网络质量，可以判断或者预知当前的网络好坏。

在网络质量要求比较高的场景中，建议在加入房间之前，先检测通话质量。

## 实现方法

在正式加入房间前，可以在本地创建两个 Client，然后一个客户端发布流、一个客户端订阅流，进入同一个测试用的房间。  
其中发布流用来测上行网络的连接状况，另一路订阅流测下行网络的连接状况。

- 1、调用 client.publish 方法发布一路流后，你可以调用 getNetworkStats 方法、getAudioStats 方法、getVideoStats 方法，获取上行网络连接数据。  
  可以通过反馈的 getNetworkStats RTT 值大致判断上行网络的质量：

  - [0,100) 上行网络质量好
  - [100,200) 上行网络质量较差
  - ≧ 200 上行网络质量很差

- 2、调用 client.subscribe 成功订阅第二路流后，你也可以调用 getNetworkStats 方法、getAudioStats 方法、getVideoStats 方法 方法获取下行网络连接数据。  
  可以通过 NetworkStats RTT 值大致判断下行网络的质量：
  - [0,200) 下行网络质量好
  - [200,400) 下行网络质量较差
  - ≧ 400 下行网络质量很差


## 示例代码

```
import sdk, { Client } from "urtc-sdk";

let client = new Client(AppId, Token, ClientOptions);

client.getNetworkStats(StreamId, (NetworkStats) => {
  const { rtt } = NetworkStats;
  console.log(`Stream accessDelay : ${rtt}ms`)
},  (error) => {
  console.log(error);
})

client.getVideoStats(StreamId, (AudioStats) => {
  const { br, lostpre, frt, w, h, mime } = NetworkStats;
  console.log(`Stream  br : ${br}`)
  console.log(`Stream  lostpre : ${lostpre}`)
  console.log(`Stream  frt : ${frt}`)
  console.log(`Stream  w : ${w}`)
  console.log(`Stream  h : ${h}`)
  console.log(`Stream  mime : ${mime}`)
},  (error) => {
  console.log(error);
})

client.getAudioStats(StreamId, (AudioStats) => {
  const { br, lostpre, vol, mime } = NetworkStats;
  console.log(`Stream  br : ${br}`)
  console.log(`Stream  lostpre : ${lostpre}`)
  console.log(`Stream  vol : ${vol}`)
  console.log(`Stream  mime : ${mime}`)
},  (error) => {
  console.log(error);
})

```

通过 getAudioStats 方法、getVideoStats 方法获取详细的网络情况：

| getAudioStats 方法 | 反馈的参数意义                                                                            |
| ------------------ | ----------------------------------------------------------------------------------------- |
| br 码率            | 音频码率 24~48Kbps 之间可以认为代表码率稳定；<br>低于 24Kbps 时，码率过低，会影响声音质量 |
| lostpre 丢包率     | 丢包率在 10%以内，可以认为网络稳定；<br>大于 10%时，网络丢包严重                          |
| vol 声音大小       | 声音大小持续在 1-100 之间变化，可以认为声音正常；<br>持续小于 20 时，音量可能过小         |
| mime 编码格式      | 固定为 opus                                                                               |

| getVideoStats 方法 | 反馈的参数意义                                                                                                                                 |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| br 码率            | 视频根据不同参数情况，码率范围不一；<br>可以根据 查看 [不同参数的视频码率](https://github.com/ucloud/urtc-sdk-web#getsupportprofilenames) 要求 |
| lostpre 丢包率     | 丢包率在 10%以内，可以认为网络稳定；<br>大于 10%时，网络丢包严重                                                                               |
| frt 帧率           | 视频根据不同参数情况，帧率不一                                                                                                                 |
| w 视频宽度         | 视频根据不同参数情况，宽高不一                                                                                                                 |
| h 视频高度         | 视频根据不同参数情况，宽高不一                                                                                                                 |
| mime: 编码格式     | 'vp8' 或 'h264'             

## 开发注意事项

- 调用 NetworkStats 方法获取的网络连接统计数据，部分可能存在延迟。
