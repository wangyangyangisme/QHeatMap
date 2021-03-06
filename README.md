QHeatMap
========

Generate Heat map in Qt.

Screen Capture
--------------

![Screen Capture 1](https://github.com/pbesedm/QHeatMap/blob/master/captures/capture-1.png?raw=true)

![Screen Capture 2](https://github.com/pbesedm/QHeatMap/blob/master/captures/capture-2.png?raw=true)





热图有时候叫热区图或者热力图，都是用于表现某种事物密集度的图形化显示。 我写的这个没有画底图，不然会更有趣，比如一个键盘，一张房屋平面图，或者一张Google地图，拿它做什么用，完全取决于你的需要。

一、主要用途 

1、网站设计，可以帮助设计人员知道用户喜欢点哪些地方，对哪些东西感兴趣，以此来改良设计，改善用户体验，百度和腾讯都有提供此类有偿服务。 对位置不敏感，就比如有张全身照的美女图，通过统计就可以知道别人最喜欢看美女的哪个部位，是不是很有意思。 

2、模拟分布，比如气象云图、无线射频信号的分布等等，都可以用这个来做。

3、众多例子：http://www.patrick-wied.at/static/heatmapjs/showcases.html

二、原理 

1、首先可以参考下面几个链接 

JavaScript:  [一个用canvas画热力图的利器] http://www.cnblogs.com/bdqlaccp/archive/2012/09/12/2681518.html 

jQuery:  [浅谈Heatmap] http://huoding.com/2011/01/04/39 

Python:  [pyHeatMap : 使用Python绘制热图的库] http://oldj.net/article/python-heat-map/

2、基本原理： 

A、创建一个跟图片大小或者网页或者窗口一样大小的二维数组（可用一维实现），例如图片分辨率为1024x768，则你的二维数组为768行，1024列，数据所有元素均初始化为0。 

B、准备两张同样大小的画布（在Qt下可用QImage，在JavaScript下可用canvas，在Python下可用PIL库的Image），一张用于绘制灰度渐变圆圈（假设为  Alpha_Canvas），一张用于着色后显示输出（Output_Canvas）。 

C、准备一个调色板，调色板从0到255填充上线性渐变的颜色，这个颜色可根据需要自由指定，调色板主要用于着色操作中取颜色。 

D、我们假设在画布上点击一下算是采一个点，则点击一下对应的数组元素的值增加1，用于标识该位置（又）被点击了一次。 

E、我们保存一个整个数组中单个位置被点击次数最多的值（假设为：max），比如在整个画布中，（10，10）这个位置被点击了20次，而其他所有点均在该点的点击次数之下，则20被存储到该变量（max）中。这就意味着每点击一次都要拿当前坐标点的点击次数和最大点击次数相比，如果最大点击次数小于当前点击次数，则更新最大点击次数，并重新绘制整个图。 

F、选择Alpha_Canvas画布，在被点击的地方绘制一个径向渐变的圆，圆心为被点击的坐标，半径可根据需要自由设置，径向渐变圆的是灰度渐变圆圈，整个径向渐变圆圈是一个黑色圆圈，但圆心的颜色要比边界的深，而圆心的Alpha通道值是该点的被点击次数（假设为：current_count）与被点击次数最多的值之比乘以255（255要根据你的绘图环境来确定，在Qt中255表示完全不透明，0表示完全透明），即公式：current_count/max*255。 

G、选择Output_Canva画布，进行着色操作，根据上一步的圆心和半径可确定一个矩形区域，如果是重绘操作，则该矩形区域为整个图形区域。对该矩形区域进行遍历，取出Alpha_Canvas画布上每个颜色点的Alpha通道值，根据此值从调色板上取出对应的颜色，R、G、B三个值用从调色板取来的值，但Alpha通道值不能相同，在这里可以设置一个阈值，用于决定渐变开始的半径，也可以自由修改。将这四个值决定的颜色点绘制到Output_Canvas画布上，依此操作，直至遍历结束。 

	H、完成着色操作后，整个绘图过程就完毕了。

3、开源项目

JavaScript 版本：[heatmap.js] http://www.patrick-wied.at/static/heatmapjs/index.html 

Python 版本：[pyHeatMap]  https://github.com/oldj/pyheatmap 

C++/Qt版本：[QHeatMap]   https://github.com/pbesedm/QHeatMap [本人实现]

4、JavaScript、Python、C++三种实现方法都有，相信你能明白原理，不明白请回复或联系我

