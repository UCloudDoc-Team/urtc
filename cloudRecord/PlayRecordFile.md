# 录制回放
URTC 云端录制的音视频文件，会自动存放在配置好的对象存储UFile的存储空间中。
录制回放，实际上是播放UFile的录像文件的URL。

## 拼接录制文件的URL地址
录制任务开始后，会回调当前录制文件的文件名。而由于对象存储UFile的配置不同，录制文件的URL拼接参数不尽相同。

| UFile空间类型 | UFile配置 | 录制文件的URL拼接方法 |
|-|-|-|
|公开空间|未使用CDN加速| http://"存储空间域名"/"file_name".mp4 |
|公开空间|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4 |
|私有空间|未使用CDN加速| http://"存储空间域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature" |
|私有空间|使用了CDN加速| http://"CDN加速域名"/"file_name".mp4?UCloudPublicKey="UCloudPublicKey"&Expires="Expires"&Signature="Signature"  |

 - 存储空间域名，可在[UFile](https://console.ucloud.cn/ufile/ufile) 查看。
 - CDN加速域名，可在[UCDN](https://console.ucloud.cn/ucdn/ucdndomainmanage) 查看。
 - API公钥`UCloudPublicKey`，可在[UAPI](https://console.ucloud.cn/uapi/apikey) 查看。
 - API有效期`Expires`，用unix时间表示，指URL的超时时间；如果没有这个参数，表示URL永不过期。
 - API签名`Signature`，需要在后台业务服务器生成，具体的生成方法可以参照这里[]()。





## API签名的算法
UFileREST API 基于 HMAC（哈希消息身份验证码）密钥使用自定义HTTP 方案进行身份验证。
生成签名时，客户端会传入`PublicKey`、`PrivateKey`、操作API action及参数，服务端通过这些传入的参数，来生成签名返回给客户端。


Signature的伪代码计算方式如下：

```

//生成请求的 query 字典
querys = {"PublicKey" : publicKey} + {其他 query 字段}

//对 query 字典按照字典排序方式(lexicographical order)排序
querys = querys.sort()

//生成 signstring
signstring = ""
for key, value in querys {
   signstring += key + value
}

//将私钥加入签名
signstring += privateKey

//按照SHA1(RFC 3174)计算签名
signature = sha1(signstring)

//以16进制显示签名
signature = HEX(signature)

```

<!-- tabs:start -->

## ** Python **

```python

//https://github.com/ucloud/ucloud-sdk-python2/blob/master/ucloud/core/auth/_cfg.py

// -*- coding: utf-8 -*-

import hashlib
from collections import OrderedDict
from ucloud.core.typesystem import schema, fields, encoder


class CredentialSchema(schema.Schema):
    fields = {
        "public_key": fields.Str(required=True),
        "private_key": fields.Str(required=True),
    }


def verify_ac(private_key, params):
    """ calculate signature by private_key/public_key
    the keys can be found on `APIKey documentation <https://console.ucloud.cn/uapi/apikey>`__
    >>> verify_ac("my_private_key", {"foo": "bar"})
    '634edc1bb957c0d65e5ab5494cf3b7784fbc87af'
    >>> verify_ac("my_private_key", {"foo": "bar"})
    '634edc1bb957c0d65e5ab5494cf3b7784fbc87af'
    :param private_key: private key
    :param params:
    :return:
    """
    params = OrderedDict(sorted(params.items(), key=lambda item: item[0]))
    simplified = ""
    for key, value in params.items():
        simplified += str(key) + encoder.encode_value(value)
    simplified += private_key
    hash_new = hashlib.sha1()
    hash_new.update(simplified.encode("utf-8"))
    hash_value = hash_new.hexdigest()
    return hash_value


class Credential(object):
    """ credential is the object to store credential information
    the keys can be found on `APIKey documentation <https://console.ucloud.cn/uapi/apikey>`__
    it can calculate signature for OpenAPI:
    >>> cred = Credential('my_public_key', 'my_private_key')
    >>> cred.verify_ac({"foo": "bar"})
    'd4411ab30953fb0bbcb1e7313081f05e4e91a394'
    :param public_key:
    :param private_key:
    """

    PUBLIC_KEY_NAME = "PublicKey"

    def __init__(self, public_key, private_key, **kwargs):
        self.public_key = public_key
        self.private_key = private_key

    def verify_ac(self, args):
        args[Credential.PUBLIC_KEY_NAME] = self.public_key
        return verify_ac(self.private_key, args)

    @classmethod
    def from_dict(cls, d):
        parsed = CredentialSchema().dumps(d)
        return cls(**parsed)

    def to_dict(self):
        return {"public_key": self.public_key, "private_key": self.private_key}

```

## ** Java **

```java

//代码源地址：https://github.com/ucloud/ucloud-sdk-java/blob/fc19ad89ebd5367954434c0e8aae44c0245bd73b/ucloud-sdk-java-common/src/main/java/cn/ucloud/common/util/Signature.java

package cn.ucloud.common.util;

import cn.ucloud.common.pojo.Account;
import cn.ucloud.common.pojo.Param;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;


public class Signature {

    private static Logger logger = LoggerFactory.getLogger(Signature.class.getName());

    /**
     * 获取签名字符串
     *
     * @param params 参数数组，
     * @return 签名字符串
     */
    protected static String getSignature(Param[] params, Account account) {
        String signature = "";
        if (params != null) {
            // 排序
            sortParams(params);
            // 拼接参数
            String stitchParams = stitchParams(params);
            // 拼接privateKey
            stitchParams += account.getPrivateKey();
            // 签名
            signature = sha1(stitchParams);
        }
        return signature;
    }

    /**
     * 获取签名字符串
     *
     * @param params 参数列表
     * @return 签名字符串
     */
    protected static String getSignature(List<Param> params, Account account) {
        String signature = "";
        if (params != null) {
            // 排序
            sortParams(params);
            // 拼接参数
            String stitchParams = stitchParams(params);
            // 拼接privateKey
            stitchParams += account.getPrivateKey();
            // 签名
            signature = sha1(stitchParams);
        }
        return signature;
    }

    /**
     * @param params 参数数组
     * @return 签名后的参数数组
     */
    protected static Param[] getParamAfterSignature(Param[] params, Account account) {
        if (params == null) {
            params = new Param[0];
        }
        Object[] objects = insertElement2Array(params, new Param("signature", getSignature(params, account)), params.length);
        if (objects == null) {
            objects = new Param[0];
        }
        int len = objects.length;
        Param[] newParams = new Param[len];
        for (int i = 0; i < len; i++) {
            if (objects[i] instanceof Param) {
                newParams[i] = (Param) objects[i];
            }
        }
        return newParams;
    }

    /**
     * @param params 参数列表
     * @return 签名后的参数列表
     */
    protected static List<Param> getParamAfterSignature(List<Param> params, Account account) {
        if (params != null) {
            params.add(new Param("signature", getSignature(params, account)));
        }
        return params;
    }


    /**
     * 对参数按照key的升序进行排序
     *
     * @param params 参数数组
     */
    protected static void sortParams(Param[] params) {
        int num = params.length;
        for (int i = 0; i < num; i++) {
            for (int j = i + 1; j < num; j++) {
                if (params[i].getParamKey().compareTo(params[j].getParamKey()) > 0) {
                    Param param = params[i];
                    params[i] = params[j];
                    params[j] = param;
                }
            }
        }
    }


    /**
     * 参数排序
     *
     * @param params 参数列表
     */
    protected static void sortParams(List<Param> params) {
        if (params != null) {
            // 排序
            Collections.sort(params, new Comparator<Param>() {
                @Override
                public int compare(Param p1, Param p2) {
                    return p1.getParamKey().compareTo(p2.getParamKey());
                }
            });
        }
    }

    /**
     * 对参数进行url编码
     *
     * @param params 参数数组
     */
    protected static void urlEncodeParams(Param[] params) {
        if (params != null) {
            int num = params.length;
            for (int i = 0; i < num; i++) {
                try {
                    params[i].setParamValue(URLEncoder.encode(params[i].getParamValue().toString(), "utf-8"));
                } catch (UnsupportedEncodingException e) {
                    logger.error(e.getMessage());
                }
            }
        }
    }


    /**
     * 对参数进行Url编码
     *
     * @param params 参数列表
     */
    protected static void urlEncodeParams(List<Param> params) {
        if (params != null) {
            for (Param param : params) {
                try {
                    param.setParamValue(URLEncoder.encode(param.getParamValue().toString(), "utf-8"));
                } catch (UnsupportedEncodingException e) {
                    logger.error(e.getMessage());
                }
            }
        }
    }


    /**
     * 获取签名串
     *
     * @param params 签名参数
     * @return 待签名的签名串
     */
    public static String stitchParams(Param[] params) {
        StringBuilder builder = new StringBuilder();
        if (params != null) {
            int num = params.length;
            for (int i = 0; i < num; i++) {
                builder.append(params[i].getParamKey());
                builder.append(params[i].getParamValue());
            }
        }
        return builder.toString();
    }


    /**
     * 获取签名串
     *
     * @param params 参数列表
     * @return 待签名的签名串
     */
    public static String stitchParams(List<Param> params) {
        StringBuilder builder = new StringBuilder();
        if (params != null) {
            for (Param param : params) {
                if (param.getParamValue() == null){
                    continue;
                }
                builder.append(param.getParamKey());
                builder.append(param.getParamValue());
            }
        }
        return builder.toString();
    }


    /**
     * sha1加密
     *
     * @param decrypt 待加密的字符串
     * @return 加密后的字符串
     */
    public static String sha1(String decrypt) {
        try {
            MessageDigest digest = java.security.MessageDigest
                    .getInstance("SHA-1");
            digest.update(decrypt.getBytes());
            byte[] messageDigest = digest.digest();
            // Create Hex String
            return FormatUtil.formatBytes2HexString(messageDigest, false);

        } catch (NoSuchAlgorithmException e) {
            logger.error(e.getMessage());
        }
        return "";
    }

    /**
     * 数组插入元素
     *
     * @param objects 原数组
     * @param element 元素
     * @param index   插入下标
     * @return 新数组
     */
    private static Object[] insertElement2Array(Object[] objects, Object element, int index) {
        Object[] newObjs = null;
        if (objects != null) {
            if (objects[0].getClass().equals(element.getClass())) {
                int len = objects.length;
                newObjs = new Object[len + 1];
                System.arraycopy(objects, 0, newObjs, 0, index);
                newObjs[index] = element;
                System.arraycopy(objects, index, newObjs, index, len - index);
            }
        } else {
            newObjs = new Object[1];
            newObjs[0] = element;
        }
        return newObjs;
    }


}

```

## ** Go **

```go

//https://github.com/ucloud/ucloud-sdk-go/blob/master/ucloud/auth/credential.go

package auth

import (
	"crypto/sha1"
	"encoding/hex"
	"io"
	"net/url"
	"sort"
	"time"
)

// Credential is the information of credential keys
type Credential struct {
	// UCloud Public Key
	PublicKey string

	// UCloud Private Key
	PrivateKey string

	// UCloud STS token for temporary usage
	SecurityToken string

	// Time the credentials will expire.
	CanExpire bool
	Expires   time.Time
}

// NewCredential will return credential config with default values
func NewCredential() Credential {
	return Credential{}
}

// CreateSign will encode query string to credential signature.
func (c *Credential) CreateSign(query string) string {
	urlValues, err := url.ParseQuery(query)
	if err != nil {
		return ""
	}
	urlValues.Set("PublicKey", c.PublicKey)
	return c.verifyAc(urlValues)
}

// BuildCredentialedQuery will build query string with signature query param.
func (c *Credential) BuildCredentialedQuery(params map[string]string) string {
	urlValues := url.Values{}
	for k, v := range params {
		urlValues.Set(k, v)
	}
	if len(c.SecurityToken) != 0 {
		urlValues.Set("SecurityToken", c.SecurityToken)
	}

	if c.PublicKey != "" {
		urlValues.Set("PublicKey", c.PublicKey)
		urlValues.Set("Signature", c.verifyAc(urlValues))
	}
	return urlValues.Encode()
}

func (c *Credential) IsExpired() bool {
	return c.CanExpire && time.Now().After(c.Expires)
}

func (c *Credential) verifyAc(urlValues url.Values) string {
	// sort keys
	var keys []string
	for k := range urlValues {
		keys = append(keys, k)
	}
	sort.Strings(keys)

	signQuery := ""
	for _, k := range keys {
		signQuery += k + urlValues.Get(k)
	}
	signQuery += c.PrivateKey
	return encodeSha1(signQuery)
}

func encodeSha1(s string) string {
	h := sha1.New()
	_, _ = io.WriteString(h, s)
	bs := h.Sum(nil)
	return hex.EncodeToString(bs)
}

```

<!-- tabs:end -->


通过服务端生成的签名，可以与[验证签名](http://testsign2.cn-sh2.ufileos.com/signGenerator.html) 的签名对比，确认签名方法正确。
 
 