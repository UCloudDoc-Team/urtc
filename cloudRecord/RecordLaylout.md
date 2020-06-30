# 录制混流风格

服务端录制时，不需要额外的SDK，仅集成各端的SDK，就可以设置录制多路音频流和视频流，设置不同的混流风格，设置是否需要水印等等。  

混流时，支持的混流风格有：选择平铺、垂直风格样式。 实际混流的用户数少于样式中的用户数时，则显示黑色底图。

## 平铺风格

默认风格，用户平铺在底图上，最多支持9个用户画面。  

不同风格的样式如下图所示：  

![ ](/images/record/pingpu1.png)

![ ](/images/record/pingpu2.png)

![ ](/images/record/pingpu3new.png)

![ ](/images/record/pingpu4.png)

![ ](/images/record/pingpu5.png)

![ ](/images/record/pingpu6.png)

![ ](/images/record/pingpu7.png)

![ ](/images/record/pingpu8.png)

![ ](/images/record/pingpu9.png)




## 垂直风格

指定一个用户显示大视窗画面，其他用户的小视窗画面在右侧垂直排列，最多支持共9个用户画面。  

不同风格的样式如下图所示：  

1人  

![ ](/images/record/pingpu1.png)

2~5人  

![ ](/images/record/chuizhi2.png)

6~7人 

![ ](/images/record/chuizhi7.png)

8~9人 

![ ](/images/record/chuizhi9.png)

## 自定义混流布局

开启录制或者旁路推流时，`layout`选 3 ，为自定义混流布局。    
自定义混流布局参数，填在`custom`里，格式参照`RFC5707 Media Server Markup Language (MSML)`。    
代码示例如下：    

```
"custom": [ 
//

                 {
                     "region": [
                         {
                             "id": "1",
                             "shape": "rectangle",
                             "area": {
                                 "left": "0",
                                 "top": "0",
                                 "width": "1",
                                 "height": "1"
                             }
                         }
                     ]
                 },
		//混流一画面，是全屏的效果
                 {
                     "region": [
                         {
                             "id": "1",
                             "shape": "rectangle",
                             "area": {
                                 "left": "0",
                                 "top": "1/4",
                                 "width": "1/2",
                                 "height": "1/2"
                             }
                         },
                         {
                             "id": "2",
                             "shape": "rectangle",
                             "area": {
                                 "left": "1/2",
                                 "top": "1/4",
                                 "width": "1/2",
                                 "height": "1/2"
                             }
                         }
                     ]
                 }
	    //混流二画面，是左右的均分效果
            ],
```

示例的混流二画面，左右均分的效果如下:    
![ ](/images/record/layout_custom_2.png)


自定义混流布局参数，说明如下：

|参数	|描述|
|-|-|
|region	| 每个region代表一种画面风格，多个region代表不同画面的效果 |
|id	| 最大可以混流17路画面，可设置 1 - 17 ，正常是顺序添加流。<br>1 画面也是主画面，可以通过`mainviewuid`指定主画面的流 |
|shape	| 只能设置为 `rectangle` ，矩形 |
|left	|画布上该画面左上角的横坐标的相对值，用分数表示，范围是 [0,1]。<br>从左到右布局，0 在最左端，1 在最右端，<br>示例中混流二画面，1/2 表示画面的左上角的顶点在画布一半的位置。 |
|top	|画布上该画面左上角的纵坐标的相对值，用分数表示，范围是 [0,1]。<br>从上到下布局，0 在最上端，1 在最下端，<br>示例中混流二画面，1/4 表示画面的左上角的顶点在画布1/4的位置。 |
|width	|画面宽度的相对值，用分数表示，取值范围是 [0,1]。<br>1 表示画面宽度与画布一致，示例中混流二画面，1/2 表示画面宽度占画布的一半 |
|height	|画面高度的相对值，用分数表示，取值范围是 [0,1]。<br>1 表示画面高度与画布一致，示例中混流二画面，1/2 表示画面高度占画布的一半 |

![ ](/images/record/layout_custom_define.png)




