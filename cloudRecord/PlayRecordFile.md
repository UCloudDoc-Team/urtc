# 录制回放
URTC 云端录制的音视频文件，会自动存放在配置好的对象存储 US3的存储空间中。录制回放，实际上是播放US3的录像文件的URL。    
录制任务开始后，会回调当前录制文件的文件名。而由于对象存储US3的配置不同，录制文件的URL拼接参数不尽相同。


## 获取对象存储 US3公开空间的URL地址
| 对象存储的配置 | 录制文件的URL |
|-|-|
|未使用CDN加速| http://"对象存储空间域名"/"file_name".mp4 |
|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4 |

 - 存储空间域名，可在[US3](https://console.ucloud.cn/ufile/ufile) 查看，也可以通过"bucket存储空间名称.region地域.ufileos.com"拼接而成。
 - CDN加速域名，可在[UCDN](https://console.ucloud.cn/ucdn/ucdndomainmanage) 查看。
 - file_name，为录制任务开始时，返回的文件名。

## 获取对象存储 US3私有空间的URL地址
| 对象存储的配置 | 录制文件的URL |
|-|-|
|未使用CDN加速| http://"存储空间域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature" |
|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature"  |

 - API公钥`UCloudPublicKey`，可在[UAPI](https://console.ucloud.cn/uapi/apikey) 查看。
 - API有效期`Expires`，用unix时间表示，指URL的超时时间；如果没有这个参数，表示URL永不过期。
 - API签名`Signature`，需要在后端服务端生成，具体的生成方法可以参照[US3的API签名方法](ufile/api/authorization)。

推荐在后端服务端直接部署**US3 SDK**。通过`GetPrivateURL`接口，给`US3 SDK`传入`文件名key`、`有效期Expires`，即获取私有空间的URL地址。
 - US3 存储的`文件名key`，为 `录制任务开始时返回的文件名.mp4`。
    
> 可以从这里获取[US3 SDK](ufile/tools/sdk)，除了`JS SDK`其他均可以直接在服务端使用。    
> 使用SDK时，注意要先在配置文件填写`bucket`、`PublicKey`、`PrivateKey`、`存储空间域名`。    

 
 
