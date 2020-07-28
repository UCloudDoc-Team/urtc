# 自动断网重连

在切换网络或者网络较差，导致客户端断网时，SDK会自动重连服务器，让用户不掉线，通话不间断。

<!-- tabs:start -->

## ** Web **

### 实现方法

通过on事件监听'stream-reconnected'，监听事件返回函数的参数为 {previous: Stream, current: Stream}，其中 previous 是重连前的发布/订阅流，current 是重连后的发布/订阅流， 通过重连前、后的流来做video的重新渲染。

### 示例代码
```js
client.on('stream-reconnected', ({ previous: oldStream, current: newStream }) => {
  //oldStream是重连之前的流，newStream是重连之后的流
})    
```
### 开发注意事项

请使用 current 来更新业务代码中的缓存。

## ** Windows **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  

## ** Android **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  




## ** iOS **

### 实现方法

balabala……    

### 示例代码

balabala……    

### 开发注意事项

balabala……  



<!-- tabs:end -->
