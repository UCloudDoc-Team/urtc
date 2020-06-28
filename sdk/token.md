# Token生成指导
## 1. 参考实现代码
 - 测试环境中，可以通过客户端自行生成`Token` ，以便尽快开始集成测试。
 - 生产环境中，需要在后台服务器中部署`Token` 服务。客户端加入房间之前，向后台服务器申请`Token` ，以保证`Token` 的安全性。
 
生成`Token`时，客户端会传入`userID`、`roomID`、`AppID`，服务端通过这些传入的参数和`AppKey`生成`Token` 。
>  `userID`，自定义的用户id；     
>  `roomID`，自定义的房间id；     
>  `AppID`，UCloud控制台创建项目获取到的AppID；    
>  `AppKey`，UCloud控制台创建项目获取到的AppKey；    

这里提供几种参考代码，供直接使用。

<!-- tabs:start -->

## ** Go **

  - Go 参考代码如下

```go
package authcenter
import (
    "crypto/hmac"
    "crypto/sha1"
    "encoding/base64"
    "encoding/hex"
    "encoding/json"
    "errors"
    "fmt"
    "strings"
)

func GenerateRoomToken(uid, appId, roomId, appCertificate string, unixTs int64, randomInt uint32) (string, error) {
    mapCombine := make(map[string]interface{})
    mapCombine["user_id"] = uid   //uid为自定义的用户id
    mapCombine["room_id"] = roomId   //roomId为自定义的房间id
    mapCombine["app_id"] = appId    //appid为UCloud控制台上创建URTC应用时生成的appid
    jsonCombine, err := json.Marshal(mapCombine)
    if err != nil {
        return "", errors.New("SYSTEM_ERROR")
    }

    singnatureStr := Generate(uid, appId, appCertificate, roomId, unixTs, randomInt)
    encodeStr := base64.StdEncoding.EncodeToString([]byte(jsonCombine))
    strCombineToken := encodeStr + "." + singnatureStr

    return strCombineToken, nil
}

func Generate(uId, appID, appCertificate, roomId string, unixTs int64, randomInt uint32) string {
    return generateDynamicKey(uId, appID, appCertificate, roomId, unixTs, randomInt)
}

func generateDynamicKey(uId, appID, appCertificate, roomId string, unixTs int64, randomInt uint32) string {
    unixTsStr := fmt.Sprintf("%010d", unixTs)
    randomIntStr := fmt.Sprintf("%08x", randomInt)
    signature := generateSignature(uId, appID, appCertificate, roomId, unixTsStr, randomIntStr)
    buffer := strings.Join([]string{signature, unixTsStr, randomIntStr}, "")
    return buffer
}

func generateSignature(uId, appID, appCertificate, roomId, unixTsStr, randomIntStr string) string {
    buffer := strings.Join([]string{uId, appID, unixTsStr, randomIntStr, roomId}, "")
    signature := hmac.New(sha1.New, []byte(appCertificate))
    signature.Write([]byte(buffer))
    return hex.EncodeToString(signature.Sum(nil))
}
```

## ** Java **
  - Java 参考代码如下

```java
import java.io.ByteArrayOutputStream;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64 ;

public class AuthToken {
    // 此方法生成token
    public static String generateToken(String uid, String roomid, String appid, String seckey){
        String token = "";
        try {
			String headerjson = "{" + "\"user_id\""+":"+ "\"" +uid  +"\""+","+ "\"room_id\""+":"+ "\""+ roomid+ "\""+","+ "\"app_id\"" +":"+ "\""+ appid + "\""+ "}" ;
		  //uid为自定义的用户id
                  //roomId为自定义的房间id
                  //appid为UCloud控制台上创建URTC应用时生成的appid
			final Base64.Encoder encoder = Base64.getEncoder();
			final byte[] textByte = headerjson.getBytes("UTF-8");
			final String base64header = encoder.encodeToString(textByte);
			long rannum = System.currentTimeMillis()/1000;
			long times = System.currentTimeMillis()/1000;
		
            String sign = generate(uid, appid, seckey,roomid, (int)times, (int)rannum);
			token = base64header+"."+sign ;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return token ;
    }
    
    public static String generate(String uID,String appID, String appCertificate, String roomID, int unixTs, int randomInt) throws Exception {
        String unixTsStr = ("0000000000" + Integer.toString(unixTs)).substring(Integer.toString(unixTs).length());
        String randomIntStr = ("00000000" + Integer.toHexString(randomInt)).substring(Integer.toHexString(randomInt).length());
        String signature = generateSignature(uID,appID, appCertificate, roomID, unixTsStr, randomIntStr);
        return String.format("%s%s%s", signature, unixTsStr, randomIntStr);
    }

    private static String generateSignature(String uID,String appID, String appCertificate, String roomID, String unixTsStr, String randomIntStr) throws Exception {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        baos.write(uID.getBytes());
        baos.write(appID.getBytes());
        baos.write(unixTsStr.getBytes());
        baos.write(randomIntStr.getBytes());
        baos.write(roomID.getBytes());
        byte[] sign = encodeHMAC(appCertificate, baos.toByteArray());
        return bytesToHex(sign);
    }

    static byte[] encodeHMAC(String key, byte[] message) throws NoSuchAlgorithmException, InvalidKeyException {
        return encodeHMAC(key.getBytes(), message);
    }

    static byte[] encodeHMAC(byte[] key, byte[] message) throws NoSuchAlgorithmException, InvalidKeyException {
        SecretKeySpec keySpec = new SecretKeySpec(key, "HmacSHA1");

        Mac mac = Mac.getInstance("HmacSHA1");
        mac.init(keySpec);
        return mac.doFinal(message);
    }

    static String bytesToHex(byte[] in) {
        final StringBuilder builder = new StringBuilder();
        for (byte b : in) {
            builder.append(String.format("%02x", b));
        }
        return builder.toString();
    }
		
}
```

## ** Node.js **

 - Node.js 参考代码如下（需要 base64.js 和  sha1.js ）

```js
import {
    Base64
} from './base64';
import jsSHA from './sha1';

generateToken(obj) {
    const _that = this;
    return new Promise(function (resolve, reject) {
        let header = Base64.encode(JSON.stringify(obj));
        let time = Math.round(Date.now() / 1000);
        let randNum = _that.randNum(8);
        let string = obj.user_id + obj.app_id + time + randNum + obj.room_id;
	//uid为自定义的用户id
        //roomId为自定义的房间id
        //appid为UCloud控制台上创建URTC应用时生成的appid
        let shaObj = new jsSHA("SHA-1", 'TEXT');
        console.log(obj.appkey)
        log(obj.appkey)
        shaObj.setHMACKey(obj.appkey, 'TEXT');
        shaObj.update(string)
        let hash = shaObj.getHMAC("HEX");
        let signture = hash + time + randNum;
        let token = header + '.' + signture;
        // _that.getUrl(token);
        resolve(token);
        if (obj.room_id !== undefined) {
            // resolve('get token success');
        } else {
            reject('get token error')
        }
    })
}
```

```js
generateToken({
    app_id: appId,//控制台创建项目获取到的appid
    room_id: roomId,//自定义的房间id
    user_id: userId,//自定义的用户id
    appkey: appkey//控制台创建项目获取到的appkey
}).then(function(data) {
    //返回当前用户的token 
}).catch(function(err){
    //报错信息 
})
```

## ** PHP **

  - PHP 参考代码如下

```php

   class AuthToken {

    // 此方法生成token
    public function generateToken($uid, $roomid, $appid, $appkey){
        $token = "";
        try {
            $header = ['user_id'=>$uid, 'room_id'=>$roomid, 'app_id'=>$appid];
              //uid为自定义的用户id
              //roomId为自定义的房间id
              //appid为UCloud控制台上创建URTC应用时生成的appid
            $headerjson = json_encode($header);
            $base64headerjson = base64_encode($headerjson);
            $time_stamp = time();  
            $rand_num = dechex($time_stamp);
            $sign = $this->generateSignature($uid, $appid, $appkey,$roomid, $time_stamp, $rand_num);
            $token = $base64headerjson.".".$sign;
        } catch (Exception $e) {
            echo $e->getMessage();
        }
        return $token ;
    }


    private function generateSignature($uid, $appid, $appkey,$roomid, $time_stamp, $rand_num){

        $plain_str = $uid.$appid.$time_stamp.$rand_num.$roomid;
        $encrypted_str = bin2hex(mhash(MHASH_SHA1,$plain_str,$appkey));
        return $encrypted_str.$time_stamp.$rand_num;
    }

}

```

## ** Python **

  - Python 参考代码如下

```python
# -*- coding: utf-8 -*-

import hmac
import base64
import random
import time
from hashlib import sha1

app_id_str = "xxx"
app_secret_str = "xxx"

// appid、app_secret为UCloud控制台上创建URTC应用时生成的appid、appkey。
// user_id为自定义的用户id，room_id为自定义的房间id

class GenerateToken(object):
    def __init__(self):
        self.app_id = app_id_str
        self.app_secret = app_secret_str

    @staticmethod
    def get_headerbase64(app_id, user_id, room_id):
        params_data = """{"user_id":"%s","room_id":"%s","app_id":"%s"}""" % (
            str(user_id), str(room_id), str(app_id))
            
        headerbase64 = str(base64.b64encode(params_data.encode("utf-8")), "utf-8")
        return headerbase64

    @staticmethod
    def get_random_hex():
        random_int = random.randint(10000000, 99999999)
        return str(random_int)

    @staticmethod
    def get_timestamp():
        timestamp = round(time.time())
        return str(timestamp)

    @staticmethod
    def get_encrypt_hmac_sha1(app_secret, data):
        sha1_str = hmac.new(app_secret.encode(encoding="utf-8"), data.encode(encoding="utf8"), sha1)
        secret = sha1_str.hexdigest()
        return secret

    def generate_sign(self, user_id, room_id):
        base64_str = self.get_headerbase64(self.app_id, user_id, room_id)
        timestamp = self.get_timestamp()
        random_str = self.get_random_hex()
        list_str = [user_id, self.app_id, timestamp, random_str, room_id]
        strformat = "".join(list_str)
        sign = self.get_encrypt_hmac_sha1(self.app_secret, strformat)
        signture = "{}{}{}".format(sign, timestamp, random_str)
        token = "{}.{}".format(base64_str, signture)
        return token
```

<!-- tabs:end -->

## 2. Token的使用流程图

![ ](/images/sdk/liuch.png)

 - 1. 加入房间之前，客户端去向后台服务器，申请 `Token` ；
 - 2. 后台服务器返回 `Token` 给客户端；
 - 3. 加入房间时，客户端将 `Token` 传给URTC SDK的相关接口；
 - 4. URTC后台服务器验证 `Token` 是否有效，允许客户端加入房间或者返回错误信息。
 

## 3. Token生成方法详解

### 3.1 创建项目生成AppID及AppKey

创建项目生成AppID及AppKey的具体方法，可以查看 [快速开始](urtc/quick)。

AppKey是保证AppID的秘钥，请妥善保管，避免泄露以及公网明文传输。

### 3.2 sha1算法生成Token

**Token 生成规则**

sha1算法利用AppID及AppKey生成Token，Token采用类jwt格式：分为头部和数据载荷，形式：header（头部）.signture(数据载荷)。  

#### 3.2.1 生成header
 
header 部分采用为json 字符串，然后进行base64 编码。

1. json字符串格式   

```
Jsonmsg = {
"user_id"： uid，
"room_id"：roomId，
"app_id"：appId
}
header=base64(Jsonmsg) ;
```

2. 生成base64编码


```
Headerbase64=base64(jsonmsg) ;
```

#### 3.2.2 生成signature

1. 时间戳获取

时间戳为UTC时间，精确到秒，截取最后10位，作为最终的时间戳。    
伪代码如下：

```
unixts = getutctimes()    
unixts=format(“%10u”, unixts)    
```

2. 随机数生成

随机生成32位的无符号整形数，然后转为16进制，保持8位长度，作为随机数。    
伪代码如下：

```
random = random()    
random=format(“%08x”, random)    
```

#### 3.2.3 签名生成

1. 格式化字符串

```
strformat = format(“%s%s%d%d%s”, userid, appid, unixts, random, roomid)\\
```

2. 通过sha1 编码 加密key 为seckey

```
sign = HmacSign(appCertificate, strformat, HMAC_LENGTH);\\
```

3. 拼接加密串

```
signture = format(“%s%d%d”, sign, unixts, random)\\
```

#### 3.2.4 拼接最终的Token

```
token = header+ “.”+ signture\\
```

## 4. 验证Token正确性

后台服务器生成的`Token`，可以与 https://tools.urtc.com.cn 生成的`Token`对比校验，判断`Token`生成方法是否正确。    

## 5. 申明

Token是SDK验证APP的重要参数，是用户进入系统的身份验证手段，token 错误将无法进入系统，请确认token 生成代码正确。
