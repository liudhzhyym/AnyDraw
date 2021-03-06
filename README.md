# AnyDraw
这是一个画板应用。

###1.介绍

这是一个参照**sketchmaster**界面制作的画板，页面简捷易操作。它包含以下功能：

1. 5种画笔(包括橡皮)，每种笔有不同的特性，这也是一个可玩点。
2. 独立粗细控制。
3. 丰富的颜色选择器。预设18种颜色，及一个取色器。
4. 可更换画布背景。
5. 前进、回退控制。
6. 清除与保存到相册。

#####1.1 界面

启动后页面如下：

![主界面](http://upload-images.jianshu.io/upload_images/3913024-1e059a6f0caff095.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

页面分为三部分：

1. 导航栏
2. 画布
3. 底部工具栏

##### 1.2 功能介绍

######笔类型选择器

目前做了5种笔，分别为：圆珠笔、倾斜笔、米形笔、喷枪、橡皮擦。笔的起名不一定正确，能理解就行。

![画笔选择](http://upload-images.jianshu.io/upload_images/3913024-cbf084d2bd8f719e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 颜色选择器

目前有固定颜色与取色器两种。

![画笔选择器](http://upload-images.jianshu.io/upload_images/3913024-a19f810d9b478a79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 画布切换

其中预设了10余种画布，可以滑动更换。

![image.png](http://upload-images.jianshu.io/upload_images/3913024-504320f9419e09ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 粗细控制

为下方工具栏中央Slider。

每种笔的初始粗细与颜色可以独立控制，修改颜色后，只会修改对应笔的粗细与颜色属性。

######线条优化

对于线条的流畅性，做了优化。方法是取中点，做二次bezier，如下图所示：

![image.png](http://upload-images.jianshu.io/upload_images/3913024-19b430d2e7af794c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中绿色空心小点，是我们touchesMove时经过的point，但我们不能直接把point连接point去绘制，那样全是折线不流畅。

如果此时正画到currunt点，向前取两个点：previous2、previous1，previous2与previous1中点为mid2，previous1与currunt中点为mid1，然后以previous1为控制点，mid2向mid1绘曲线，图中绿色部分。依次向后绘，就能得到平滑曲线。

快速绘制时，此方法会导致线条显示偏移原有线路，不过一般看不出来。也考虑过用三次bizier成线，貌似线条不够自然。三次bezier参考[DEMO](https://www.zheng-hang.com/zb_users/upload/2015/04/201504121428779779603343.rar)

###### 绘制方式

采用了两种绘制方式。
1. shapeLayer上设置path
2. bitMap绘制

圆珠笔与橡皮擦是直接在shapeLayer上通过设置path实现，特点是线条流畅，缺点是不能控制线条的粗细变化。

其它笔是用bitMap实现，通过把bezier线条**大致等分**取点，然后在每个点上绘制图片。图片的大小与点的间距可以通过算法控制，每个点在bezier线条上的方向也能大致得到，这就提供给了我们无限的扩展空间。比如：毛笔的粗细变化、路径上小星星的朝向。

### 待优化

1. 目前bezier曲线均分的算法不够精确。
2. 每段bezier结束与下一段bezier间衔接有BUG，需要优化。
3. 曲线上粗细变化需要逐渐过渡，目前是突然性的。
4. bezier上点的方向还未判定。

喜欢的话，请star，谢谢支持。欢迎提出好的建议，fork共同改进代码。

我的简书地址：http://www.jianshu.com/p/deb107a1ec36
