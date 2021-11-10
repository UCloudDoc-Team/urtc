# RTMP推流

URTC支持`RTMP`推流作为一个发送源，发送到`RTC`房间内。所有用户可以一起实时收听/观看该媒体流，并实时互动。     

![](/images/rts.png)

支持的推流方式有：
 - OBS推`RTMP`音视频源
 - 网络摄像头推`RTMP`音视频源
 - 自定义的`RTMP`服务推音视频源

支持的编码格式：音频 `AAC`，视频 `H.264`。    
纯音频流也可作为在线媒体流输入房间。     

`RTMP`推流地址拼接方式：    
`rtmp://push.urtc.com.cn/live/stream_id?room=xxx&app=xxx&user=xxx&token=xxx`

?> 说明    
?>  room：房间号码，由业务决定。加入房间，就可开始实时通话、互动直播，同一个`AppID`同一个`room`的用户才可以通话。    
?>  app：是[UCloud控制台](https://console.ucloud.cn/)创建`URTC`应用时生成的随机字符串，是`URTC`后台服务器识别`URTC`客户身份的唯一标识。     
?>  user：用户号码，由业务决定。客户端加入房间时的身份标识，同一个房间内的用户必须唯一。    
?> token：鉴权码，必须使用合法的鉴权码，才能正常的推流，鉴权码的生成规则查看[Token生成指导](https://docs.ucloud.cn/urtc/sdk/token)，也可以直接通过 https://tools.urtc.com.cn/ 临时获取`token`用来测试。    

