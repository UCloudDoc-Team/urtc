# 视频快照

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。    


<!-- tabs:start -->

## ** Web **

### 实现方法

将指定的发布（本地）/订阅（远端）流截屏用于页面展示，或下载截屏图片   

### 示例代码

```js
client.snapshot({
  streamId?: string,             // 选填，发布/订阅流的 ID，不填时，为第一条发布流，
  download?: boolean 或 string  // 选填，是否要下载图片，或指定下载图片的文件名，传 true 时，可将截屏下载到本地（文件名自动生成），传非空字符串时，将会以该字符串命名下载时保存到本地的图片名，不传或传 false 或空字符串时，都将不下载图片
}, function(ImgString) {
        //ImgString: string 类型，是图片转化的 base64 编码的 [Data URLs](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/data_URIs)，可将其赋值给 Image 元素 - 详见 [HTMLImageElement](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement) 的 src 属性。
}, function(Err){
        //方法调用失败时执行的回调函数
});
```


## ** Android **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。  

### 示例代码

view传本地的就是本地截图，远端的view就是远端的截图。    


```java
    private UCloudRTCScreenShot mUCloudRTCScreenShot = new UCloudRTCScreenShot() {
        @Override
        public void onReceiveRGBAData(ByteBuffer rgbBuffer, int width, int height) {
            final Bitmap bitmap = Bitmap.createBitmap(width * 1, height * 1, Bitmap.Config.ARGB_8888);
            bitmap.copyPixelsFromBuffer(rgbBuffer);
            String name = "/mnt/sdcard/urtcscreen_"+System.currentTimeMillis() +".jpg";
            File file = new File(name);
            try {
                FileOutputStream out = new FileOutputStream(file);
                if (bitmap.compress(Bitmap.CompressFormat.JPEG, 100, out)) {
                    out.flush();
                    out.close();
                }
            } catch (FileNotFoundException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            }
            Log.d(TAG, "screen shoot : " + name);
            ToastUtils.shortShow(RoomActivity.this,"screen shoot : " + name);
        }
    };
	
	private void addScreenShotCallBack(View view){		
        if(view instanceof UCloudRtcSdkSurfaceVideoView){//view类型是UCloudRtcSdkSurfaceVideoView类型
            ((UCloudRtcSdkSurfaceVideoView)view).setScreenShotBack(mUCloudRTCScreenShot);
        }else if(view instanceof UCloudRtcRenderView){//view类型是UCloudRtcRenderView类型
            ((UCloudRtcRenderView)view).setScreenShotBack(mUCloudRTCScreenShot);
        }else if(view instanceof TextureView) {//view类型是TextureView类型
            ((TextureView)view).setScreenShotBack(mUCloudRTCScreenShot);
        }
    }
```

## ** iOS **

### 示例代码

view传本地的就是本地截图，远端的view就是远端的截图。  

```
    UIGraphicsBeginImageContext(view.frame.size);
    [view drawViewHierarchyInRect:view.bounds afterScreenUpdates:NO];
    UIImage *image =  UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    UIImageWriteToSavedPhotosAlbum(image, self, @selector(image:didFinishSavingWithError:contextInfo:), NULL);
```


<!-- tabs:end -->

## 开发注意事项

balabala……  

