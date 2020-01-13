# 视频快照

<!-- tabs:start -->

# ** Web **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。    

## 实现方法

balabala……    

## 示例代码

balabala……    

## 开发注意事项

balabala……  

# ** Windows **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。      


# ** Android **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。  

## 示例代码

view传本地的就是本地截图，远端的view就是远端的截图。    

```js
private void addScreenShotCallBack(UCloudRtcSdkSurfaceVideoView view){
        view.setScreenShotBack(new UcloudRTCSceenShot() {
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
        });
}
```

# ** iOS **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。    

## 示例代码

view传本地的就是本地截图，远端的view就是远端的截图。  

```
    UIGraphicsBeginImageContext(view.frame.size);
    [view drawViewHierarchyInRect:view.bounds afterScreenUpdates:NO];
    UIImage *image =  UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    UIImageWriteToSavedPhotosAlbum(image, self, @selector(image:didFinishSavingWithError:contextInfo:), NULL);
```

# ** macOS **


视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。    


# ** Electron **

视频快照，可以将采集的本地视频或者接收的远端视频，截图保存到本地。        


<!-- tabs:end -->

