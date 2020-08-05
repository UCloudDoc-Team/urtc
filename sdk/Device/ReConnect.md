# 断网自动重连

在切换网络或者网络较差，导致客户端断网时，SDK会自动重连服务器，让用户不掉线，通话不间断。

<!-- tabs:start -->

## ** Web **

断网重连时，会触发两种事件：connection-state-change, stream-reconnected。
1. connection-state-change 事件将通知与信令服务器的整体的连接的状态的变更，断网重连时，会有 'OPEN' => 'RECONNECTING'=> 'OPEN' 状态的变化;
2. stream-reconnected 事件将通知每条具体的流的重连。

具体请参考 https://github.com/ucloud/urtc-sdk-web#client-on 中的说明。

### 实现方法

1. 通过on事件监听'connection-state-change'，监听事件返回函数的参数为 {previous: State, current: State}，其中 previous 与 current 分别为连接状态变化前、后的值，页面中可通过此变化的值为提示用户当前的连接状态，不需要提示用户连接状态时，此事件可不处理。

2. 通过on事件监听'stream-reconnected'，监听事件返回函数的参数为 {previous: Stream, current: Stream}，其中 previous 是重连前的发布/订阅流，current 是重连后的发布/订阅流， 通过重连前、后的流来做video的重新渲染，请务必处理此事件，用于本地缓存的更新。

### 示例代码

```js
this.client.on('connection-state-change', (states) => {
  // 连接状态由 states.previous 变更为了 states.current
});
```

```js
client.on('stream-reconnected', ({ previous: oldStream, current: newStream }) => {
  //oldStream是重连之前的流，newStream是重连之后的流
});
```

### 开发注意事项

请务必处理 'stream-reconnected' 事件，并使用 current 来更新业务代码中的缓存，进而对其进行播放或其他处理。而 'connection-state-change' 事件请根据业务需求进行酌情处理。

## ** Windows **

断网重连时，用户无需实现重连，sdk内部自动进行断网重连，重连结果通过以下接口告知，用户可以实现以下空接口来获得通知。

### 示例代码

```cpp
//断线无法恢复
virtual void onServerDisconnect()

//断线重连中
virtual void onRejoining(const char* uid, const char* roomid)

//断线重连成功加入房间
virtual void onReJoinRoom(const char* uid, const char* roomid)  
```

### 开发注意事项

不要在回调得接口中做耗时操作。

## ** Android **

sdk自主完成断网重连，重连结果通过回调接口通知app。

### 示例代码

```java
//与服务器断开
public void onServerDisconnect() ;

//断线重连中
public void onRejoiningRoom(String roomid) ;

//断线重连成功加入房间
public void onRejoinRoomResult(String roomid) ; 
```

### 开发注意事项

回调接口避免使用耗时操作。 




## ** iOS **

sdk自主完成断网重连，重连结果通过回调接口通知app。   

### 示例代码

在配置参数时，可以自定义更改重连的次数、时间间隔。

 ```objectivec  
// 设置断网重连次数 默认为：10次
    sdkEngine.reConnectTimes = 20;
// 设置重连时间间隔，默认60秒钟
    sdkEngine.overTime = 60; 
```

在房间内，可以监听重连状态：

```objectivec 
//重连结果监听
-(void)uCloudRtcEngine:(UCloudRtcEngine *)engine connectState:(UCloudRtcConnectState)connectState{}
```


<!-- tabs:end -->
