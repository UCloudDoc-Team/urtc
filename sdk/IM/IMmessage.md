# IM自定义消息

<!-- tabs:start -->

# ** Web **

## 实现方法

balabala……    

## 示例代码

balabala……    

## 开发注意事项

balabala……  

# ** Windows **

# ** Linux **

# ** Android **

## 示例代码

```js
 mSendMsgBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                try {
                    JSONObject content = new JSONObject();
                    content.put("title","test customer");
                    imEngine.pushCustomerMessage("test",content.toString());
                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
        });
```
# ** iOS **

## 示例代码

```
    ....
    [self.imEngine pushCustomContent:parameters completionHandler:^(NSData * _Nullable data, NSError * _Nullable error) {}];
```

# ** macOS **

# ** Electron **

<!-- tabs:end -->

