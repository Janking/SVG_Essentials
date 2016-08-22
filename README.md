SVG精髓
## 第一章 入门指南
### 1. 图形系统
计算机中描述图形信息的两大图形系统：栅格图形和矢量图形。栅格图形中图形被表示为图片元素或者像素的长方形数组。矢量图形中图形被描述为一系列几何形状，通过矢量图形阅读器在指定的坐标集上绘制形状。
### 2. SVG(Scalable Vector Graphics)
SVG是一种XML应用，用来表示矢量图形。所有的图形有关信息被存储为纯文本，具有XML的开放性、可移植性和可交互性。当前稳定的XML和SVG版本都为1.1

SVG文档结构是标准的XML文档，根元素svg定义图形的大小，根元素中包含各种的形状元素。SVG允许使用单独的属性指定元素的样式。

SVG使用g元素对图形进行分组，使用use元素实现元素的复用。

## 第二章 在网页中使用SVG
### 1. 将SVG作为图像
将svg作为图像包含在HTML标记的img元素内，但是这样有一定的局限性：**SVG转为栅格图像时与主页面分离，并且无法在两者之间通信(SVG渲染过程与主页面独立)。主页面上的样式对SVG无效，运行在主页面上的脚本无法感知或者修改SVG文档结构。**

在CSS中包含SVG，最常用的是background-image属性，应该避免SVG元素文件太大。

### 2. 将SVG作为应用程序
使用object元素将SVG嵌入HTML文档中，object元素的type属性表示要嵌入的文档类型，对用SVG应该是type="image/svg+xml"。object元素必须有起始标签和结束标签，这两个标签之间的内容为对象数据本身不能被渲染时显示。

## 第三章 坐标系统
### 1. 视口
视口是指文档打算使用的画布区域。在svg元素上使用width和height属性确定视口的大小，属性值可以仅仅是为数字也可以为带单位的数字(单位可以为em、ex、px、pt、pc、cm、mm和in)也可以为百分比。
### 2. 默认用户坐标
SVG阅读器会设置一个坐标系统，即原点(0,0)位于视口的左上角，x向右递增，y向下递增。这个坐标系统是一个纯粹的几何系统，点没有大小，网格线被认为是无限细。

在SVG中指定单位并不会影响其他元素中给定单位的坐标，也就是说SVG文档中各个元素的单位可以不统一。

### 3. 指定用户坐标
摒弃阅读器设置的默认用户坐标，可以自己为视口设置一个用户坐标。通过在svg元素上设置viewBox属性。

viewBox属性由4个数值组成，分别代表要叠加在视口上的最小x、最小y，宽度、高度。

既然可以对svg自定义用户坐标，那么肯定要解决SVG视口长宽比例和viewBox定义的长宽比例不同的问题以及如何对齐问题。这个时候就需要preserveAspectRatio属性了。

如果viewBox的长宽比例与视口的长宽比例不同，那么SVG可以有以下三种选择：

a. 按较小的尺寸等比例缩放图形，使图形完全填充视口

b. 按较大的尺寸等比例缩放图形，病裁减掉超出视口的部分

c. 拉伸和压缩绘图以使其恰好填充视口

preserveAspectRatio属性允许指定被缩放的图形相对视口的对齐方式,格式为preserveAspectRatio = "alignment[meet|slice]"   

默认值为"xMidYMid meet"

·***alignment***指定轴和位置，x和y方向都有min,mid,max三种方式，分别表示x和y方向的对齐方式，对齐方式由x和y组合指定，共9中方式，也就是alignment共有如下9个取值：


y\x | xMin | xMid | xMax
---|---|---|---
**yMin** | xMinYMin | xMidYMin | xMaxYMin
**yMid** | xMinYMid | xMidYMid | xMaxYMid
**yMax** | xMinYMax | xMidYMax | xMaxYMax

·***meet***说明符在图形超出视口时候会对图形适当缩小调整适配可用的空间

·***slice***说明符直接裁剪超出视口的部分

除了上述操作之外，还可以指定**preserveAspectRatio="none"**，用于在viewBox和视口宽高比不同时缩放图像，此时图像不会被等比例缩放，会被拉伸、挤压、变形。

### 4. 嵌套坐标系统

可以将另一个svg元素插入到文档中来建立一个新的视口和坐标系统，也就是说svg中可以嵌套另一个svg，每个svg都有自己独立的视口和坐标系统。

## 第四章 基本形状

### 1. 线段
line元素，使用x1,y1,x2,y2属性指定线段的起止点坐标。有如下特性:


特性 | 说明
---|---
stroke-width | 笔画宽度，坐标网格线位于笔画的正中间，可以使用css的shape-rendering值来控制反锯齿特性
stroke | 笔画颜色
stroke-opacity | 线条的不透明度
stroke-dasharray | 虚线，由一系列数字组成，数字个数为偶数(负责会自动重复一遍使其为偶数),表示线长-间隙-线长-间隙...

### 2. 矩形

rect元素，使用x,y,width,height表示一个矩形


特性 | 说明
---|---
fill | 填充颜色
fill-opacity | 填充不透明度
stroke | 边框颜色
stroke-width | 边框宽度，边框是骑在矩形边界上的，一半在矩形外，一半在矩形内 
rx/ry | 圆角矩形，最大值为矩形宽/高的一半，如果只指定了一个，则认为两个都为相同的值

### 3. 圆和椭圆

circle元素表示圆,由cx,cy,r属性界定
ellipse元素表示椭圆,由cx,cy,rx,ry界定

特性 | 说明
---|---
fill | 填充颜色
fill-opacity | 填充不透明度
stroke | 边框颜色
stroke-width | 边框宽度，边框是骑在圆的边界上的，一半在圆/椭圆外，一半在圆/椭圆内 

### 4. 多边形

polygon元素指定一个多边形,由points属性指定的一系列坐标点界定，会自动封闭

特性 | 说明
---|---
fill | 填充颜色
fill-opacity | 填充不透明度
stroke | 边框颜色
stroke-width | 边框宽度
fill-rule | 填充规则，如果多边形的边有交叉时，需要指定，可以取mozero(默认)和evenodd两个值。

fill-rule值为nonzero时的原理:判断一个点是在多边形内部还是外部时，从这个点画一条到无穷远的射线，然后数这个线和多边形的边有多少次交叉。如果交叉的边线是从右往左画，则总数加1，如果是从左往右则总数减1.如果最后总数为0则认为改点在图形外部，否则在内部。

fill-rule值为evenodd时只数射线与多边形边的交叉次数，如果为奇数则认为在多边形内部，否则认为在多边形外部。

### 5. 折线

polyline元素表示一个折线，使用points属性指定一系列点，不自动封闭图形。

### 6. 特性总结


特性 | 说明
---|---
stroke | 笔画颜色
stroke-width | 笔画宽度
stroke-opacity | 笔画不透明度
stroke-dasharray | 虚线笔画 
stroke-linecap | 笔画头的形状  butt(默认),round,square
stroke-linejoin | 图形棱角，有miter(默认),round和bevel三个取值
stroke-miterlimit | 相交处显示宽度与线宽的最大比例，默认为4
fill | 填充颜色 默认black
fill-opacity | 填充不透明度
fill-rule | 填充规则

## 第五章 文档结构

### 1. 结构和表现

SVG允许文档表现和文档结构分离，SVG支持四种方式指定表现信息：内联样式、内部样式表、外部样式表以及表现属性


表现方式 | 说明
---|---
内联样式 | 元素内部使用style属性
内部样式表 | 内部样式定义在defs元素内部
外部样式表 | 与html类似，将样式定义在css文件中，使用选择器来设置相应的元素样式
表现属性 | SVG允许以属性的形式指定表现样式，但是**表现属性的优先级最低**，如果以其他三种形式指定了相同的样式属性，则将覆盖通过表现属性指定的样式

内部样式表示例：

```
<svg width="200px" height="200px" xmlns="http://www.w3.org/2000/svg>
    <defs>
        <style type="text/css"><![CDATA[
            circle{
                fill:#ccc
            }
        ]]></style>
    </defs>
    <circle cx="10" cy="10" r="5"/>
</svg>
```

### 2. 分组和引用

g元素用来将其子元素作为一个组合，可以使文档结构更清晰。除此之外，在g标签中指定的所有样式会应用于组合内的所有子元素，可以不用在所有子元素上指定属性。

use元素用来复用图形中重复出现的元素，需要为use标签的xlink:href指定URI来引用指定的图形元素。同时还要指定x和y属性以表示组合应该移动到哪个位置。use元素并不限制只能使用同一个文件内的对象，xlink:href属性可以指定任何有效的文件或URI。

defs元素用来定义复用的元素，但是定义在defs内的元素并不会被显示，而是作为模板供其他地方使用。

symbol元素与g元素不同，symbol永远不会被显示，也可以用来指定被后续使用的元素，symbol元素可以指定viewBox和preserveAspectRatio属性。在引用时通过为use元素指定width和height属性就可以让symbol元素适配视口大小。

image可以用来包含一个完整的SVG或栅格文件。如果包含一个SVG文件，则视口会基于引用的文件的x,y,width,height属性来建立。如果包含栅格文件则会被缩放以适配该属性指定的矩形。SVG规范要求SVG阅读器支持JPEG和PNG两种栅格文件。

## 第六章 坐标系统变换

### 1. translate变换

translate变换用来对用户坐标进行平移，通过制定transform属性值来设置:transform = "translate(x,y)"。

translate工作原理:首先获取整个网络，然后将其移动到画布的新位置而不是移动所在的元素，也就是说移动的是整个坐标系统而不是元素本身。看似比移动元素复杂，其实在使用其他一系列变换时，这种移动整个坐标系的方法从数学和概念上讲，更方便。

### 2. scale变换

缩放坐标系统。transform = "scale(value)"或者transform="scale(x-value,y-value)"。

仅仅使用scale(n)变换时，网格系统的原点位置并没有变化，只是每个用户坐标都变成了原来的n倍，也就是网格变大了，因此线也会变粗(用户单位并没有变)。

*技巧：如果从其他系统传输数据到SVG，则可能必须处理使用笛卡尔坐标表示的矢量图形，在笛卡尔坐标系统中，原点位于左下角，y向上递增，x向右递增。而SVG坐标原点位于左上角，此时使用scale(1,-1)就可以完成两者之间的转换。*

**缩放变换永远不会改变图形对象的网格坐标或者笔画宽度，仅仅改变对应画布上的坐标系统网格的大小。**

### 3. rotate变换

根据指定的角度旋转坐标系统，默认的坐标系统中，角度的测量顺时针增加，0度为3点钟方向。

注意，除非另行指定，否则旋转以原点为中心。 此时可以通过平移+旋转的方式来指定旋转中心：
translate(centerX,centerY) rotate(angle) translate(-centerX,-centerY)

但是有个更简单的方式：rotate(angle,centerX,centerY)

### 5. 围绕中心点缩放

上面提到，缩放默认是以原点为基准的，这显然不能满足需求，那么可以通过如下方式指定缩放中心：

translate(-centerX*(factor-1),-centerY*(factor-1)) scale(factor)


### 6. skewX和skewY变换

这两个变换用来倾斜某个轴，一般形式为skewX(angle),skewY(angle)。这样的结果就是使得x轴和y轴不再垂直。

### 7. 矩阵变换

计算机图形学中坐标变换都通过矩阵来实现，除上述变换方法之外，还可以直接为变换指定变换矩阵，变换矩阵为matrix(a,b,c,d,e,f)，此时指定的变换矩阵为:

```
a  c  e
b  d  f
0  0  1

```
## 第七章 路径

### 1. path命令
SVG中所有基本形状都是path的简写形式，但是建议使用简写形式，因为这样可以使SVG文档更可读。

path元素更通用，可以通过制定一系列相互连接的线、弧、曲线来绘制任意形状的轮廓，这些轮廓也可以填充或者绘制轮廓线，也可以用来定义裁剪区域或蒙版。

下表为path命令总结，其中大写表示绝对坐标，小写表示相对坐标：


命令 | 参数 | 说明
---|---|---
M m | x y | 移动画笔到制定坐标
L l | x y | 绘制一条到给定坐标的线
H h | x  | 绘制一条到给定x坐标的横线
V v |  y | 绘制一条到给定y坐标的垂线
A a | rx ry x-axis-rotation large-arc sweep x y  | 圆弧曲线命令有7个参数，依次表示x方向半径、y方向半径、旋转角度、大圆标识、顺逆时针标识、目标点x、目标点y。大圆标识和顺逆时针以0和1表示。0表示小圆、逆时针
Q q | x1 y1 x y | 绘制一条从当前点到x,y控制点为x1,y1的二次贝塞尔曲线
T t | x y | 绘制一条从当前点到x,y的光滑二次贝塞尔曲线，控制点为前一个Q命令的控制点的中心对称点，如果没有前一条则已当前点为控制点。
C c | x1 y1 x2 y2 x y | 绘制一条从当前点到x,y控制点为x1,y1 x2,y2的三次贝塞尔曲线
S s | x2 y2 x y | 绘制一条从当前点到x,y的光滑三次贝塞尔曲线。第一个控制点为前一个C命令的第二个控制点的中心对称点，如果没有前一条曲线，则第一个控制点为当前的点。

路径的填充同样可以使用fill-rule属性指定填充规则，如果需要填充一个中空的形状，则只需要注意外侧路径顺逆时针方向和内侧空心区域顺逆时针方向即可。


### 2. marker元素

marker元素用来在path上添加一个标记，比如箭头之类的。

首先需要定义好marker元素，然后在path中引用，一个marker标记是一个独立的图形，有自己的私有坐标。


```
<defs>
    <marker id="marker" markerWidth="10" markerHeight="10" refX="0" refY="4" orient="auto">
        <path d="M 0 0 4 4 0 8" style="fill:none;stroke:black;"/>
    </marker>
</defs>

<path d="M 10 20 100 20 A 20 30 0 0 1 120 50 L 120 110"
    style="marker-start:url(#marker);marker-mid:url(#marker);marker-end:url(#marker);fill:none;stroke:black;"/>
```


marker属性 | 说明
---|---
markerWidth | marker标记的宽度
markerHeight | marker标记的高度
refX refY | 指定marker中的哪个坐标与路径的开始坐标对齐
orient | 自动旋转匹配路径的方向，需要设置为auto
markerUnits | 这个属性决定标记的坐标系统是否需要根据path的笔画宽度调整，如果设置为strokeWidth，则标记会自动调整大小。如果设置为useSpaceOnUse，则不会自动调整标记的大小。
viewBox preserveAspectRatio | 设置标记的显示效果，比如可以将标记的(0,0)设置在标记网格中心

path中使用marker-start,marker-mid,marker-end,marker属性来设置标记的位置。如果使用marker属性，则表示同时设置marker-start,marker-mid,marker-end三个属性。

除在path中以属性的形式设置标记外，还可以在css中设置：


```
path{
    marker:url(#marker_id);
}
```

但是此时要注意，如果id为marker_id的标记中也有path元素，则会出现自身无限引用自身的情况，因此需要说明marker中的path元素不需要添加标记：


```
path{
    marker:url(#marker_id);
}
marker#marker_id path{ //marker下的id为marker的元素下的path元素不需要marker标记
    marker:none;
}
```


## 第八章 图案和渐变

填充图形或笔画除了使用fill,stroke纯色之外，还可以使用图案和渐变填充。

### 1. 图案

使用图案填充图形，首先要定义一个水平或垂直方向的重复的图案对象，然后用它填充另一个对象或者作为笔画使用。这个图形对象呗称为"tile"(瓷砖)。

图案对象使用pattern元素定义，pattern元素内部包裹了图案的path元素。定义好之后下一个需要解决的问题是如何排列图案，那就需要使用patternUnits属性.

如果希望图案的大小基于要填充对象的大小计算，则需要设置patternUnits属性为objectBoundingBox(0到1之间的小数或百分比)，并需要指定图案左上角的x和y坐标。


```
<defs>
	<pattern id="tile" x="0" y="0" width="20%" height="20%" patternUnits="objectBoundingBox">
    	    <path d="M 0 0 Q 5 20 10 10 T 20 20" style="stroke:black;fill:none"></path>
	    <path d="M 0 0 h 20 v 20 h -20 z" style="stroke:black;fill:none"></path>
	</pattern>
</defs>
<path d="M 0 0 Q 5 20 10 10 T 20 20" style="stroke:black;fill:none"></path>
<path d="M 0 0 h 20 v 20 h -20 z" style="stroke:black;fill:none"></path>
<rect x="40" y="0" width="100" height="100" style="fill:url(#tile);stroke:black"></rect>	
<rect x="155" y="0" width="70" height="80" style="fill:url(#tile);stroke:black"></rect>
<rect x="250" y="0" width="150" height="130" style="fill:url(#tile);stroke:black"></rect>
```

![image](https://github.com/xswei/SVG_Essentials/blob/master/image/8.1.jpg)

在上图中，第一个正方形宽高都为100，百分之20刚好为图案的尺寸，因此恰好平铺。在第二个图中，正方形的宽高分别为70和80，百分之20小于20，因此图案会被截断。在第三个正方形中，宽高的百分之20大于图案的尺寸，因此图案平铺时会出现间隙。








































