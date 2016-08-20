SVG精髓
## 第一章 入门指南
### 1. 图形系统
计算机中描述图形信息的两大图形系统：栅格图形和矢量图形。栅格图形中图形被表示为图片元素或者像素的长方形数组。矢量图形中图形被描述为一系列几何形状，通过矢量图形阅读器在指定的坐标集上绘制形状。
### 2. SVG(Scalable Vector Graphics)
SVG是一种XML应用，用来表示矢量图形。所有的图形有关信息被存储为纯文本，具有XML的开放性、可移植性和可交互性。当前稳定的XML和SVG版本都为1.1

SVG文档结构是标准的XML文档，根元素<svg>定义图形的大小，根元素中包含各种的形状元素。SVG允许使用单独的属性指定元素的样式。

SVG使用<g>元素对图形进行分组，使用<use>元素实现元素的复用。

## 第二章 在网页中使用SVG
### 1. 将SVG作为图像
将svg作为图像包含在HTML标记的<img>元素内，但是这样有一定的局限性：**SVG转为栅格图像时与主页面分离，并且无法在两者之间通信(SVG渲染过程与主页面独立)。主页面上的样式对SVG无效，运行在主页面上的脚本无法感知或者修改SVG文档结构。**

在CSS中包含SVG，最常用的是background-image属性，应该避免SVG元素文件太大。

### 2. 将SVG作为应用程序
使用<object>元素将SVG嵌入HTML文档中，<object>元素的type属性表示要嵌入的文档类型，对用SVG应该是type="image/svg+xml"。<object>元素必须有起始标签和结束标签，这两个标签之间的内容为对象数据本身不能被渲染时显示。

## 第三章 坐标系统
### 1. 视口
视口是指文档打算使用的画布区域。在<svg>元素上使用width和height属性确定视口的大小，属性值可以仅仅是为数字也可以为带单位的数字(单位可以为em、ex、px、pt、pc、cm、mm和in)也可以为百分比。
### 2. 默认用户坐标
SVG阅读器会设置一个坐标系统，即原点(0,0)位于视口的左上角，x向右递增，y向下递增。这个坐标系统是一个纯粹的几何系统，点没有大小，网格线被认为是无限细。

在SVG中指定单位并不会影响其他元素中给定单位的坐标，也就是说SVG文档中各个元素的单位可以不统一。

### 3. 指定用户坐标
摒弃阅读器设置的默认用户坐标，可以自己为视口设置一个用户坐标。通过在<svg>元素上设置viewBox属性。

viewBox属性由4个数值组成，分别代表要叠加在视口上的最小x、最小y，宽度、高度。

既然可以对<svg>自定义用户坐标，那么肯定要解决SVG视口长宽比例和viewBox定义的长宽比例不同的问题以及如何对齐问题。这个时候就需要preserveAspectRatio属性了。

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

可以将另一个<svg>元素插入到文档中来建立一个新的视口和坐标系统，也就是说<svg>中可以嵌套另一个<svg>，每个<svg>都有自己独立的视口和坐标系统。

## 第四章 基本形状

### 1. 线段
<line>元素，使用x1,y1,x2,y2属性指定线段的起止点坐标。有如下特性:


特性 | 说明
---|---
stroke-width | 笔画宽度，坐标网格线位于笔画的正中间，可以使用css的shape-rendering值来控制反锯齿特性
stroke | 笔画颜色
stroke-opacity | 线条的不透明度
stroke-dasharray | 虚线，由一系列数字组成，数字个数为偶数(负责会自动重复一遍使其为偶数),表示线长-间隙-线长-间隙...

### 2. 矩形

<rect>元素，使用x,y,width,height表示一个矩形


特性 | 说明
---|---
fill | 填充颜色
fill-opacity | 填充不透明度
stroke | 边框颜色
stroke-width | 边框宽度，边框是骑在矩形边界上的，一半在矩形外，一半在矩形内 
rx/ry | 圆角矩形，最大值为矩形宽/高的一半，如果只指定了一个，则认为两个都为相同的值

### 3. 圆和椭圆

<circle>元素表示圆,由cx,cy,r属性界定
<ellipse>元素表示椭圆,由cx,cy,rx,ry界定

特性 | 说明
---|---
fill | 填充颜色
fill-opacity | 填充不透明度
stroke | 边框颜色
stroke-width | 边框宽度，边框是骑在圆的边界上的，一半在圆/椭圆外，一半在圆/椭圆内 

### 4. 多边形

<polygon>元素指定一个多边形,由points属性指定的一系列坐标点界定，会自动封闭

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

<polyline>元素表示一个折线，使用points属性指定一系列点，不自动封闭图形。

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





