# 播放直播流

不同直播服务平台的直播流域名规则，不完全相同，这里只以UCloud的 [直播云ULive](https://docs.ucloud.cn/ulive/README) 举例。    

示例： 推流域名：publish.company.com，接入点：test。rtmp播放域名：rtmp.company.com， hls播放域名：hls.company.com 。    
则    
推流地址为：rtmp://publish.company.com/test/{Roomid}       
rtmp播放地址为：rtmp://rtmp.company.com/test/{Roomid}    
hls播放地址为：http://hls.company.com/test/{Roomid}/playlist.m3u8    

> 播放直播流产生的流量，按照 [直播云ULive产品价格](https://docs.ucloud.cn/ulive/charge) 而定。
