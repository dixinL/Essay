# SVG 详解

##### [Essay](https://dixinl.github.io/Essay/)

## 什么是 SVG

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用来定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有所损失
- SVG 是万维网联盟的标准
- SVG 与诸如 DOM 和 XSL 之类的 W3C 标准是一个整体
- SVG 于 2003 年 1 月 14 日成为 W3C 推荐标准

## SVG 的优势

与其他图像格式相比，使用 SVG 的优势在于：

- SVG 可被非常多的工具读取和修改（比如记事本）
- SVG 与 JPEG 和 GIF 图像比起来，尺寸更小，且可压缩性更强。
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大
- SVG 图像中的文本是可选的，同时也是可搜索的（很适合制作地图）
- SVG 可以与 Java 技术一起运行
- SVG 是开放的标准
- SVG 文件是纯粹的 XML

SVG 的主要竞争者是 Flash。

与 Flash 相比，SVG 最大的优势是与其他标准（比如 XSL 和 DOM）相兼容。而 Flash 则是未开源的私有技术。

## 如何使用 SVG

SVG 文件必须使用 .svg 后缀来保存。

> ```html
> <!--包含了 XML 声明,standalone 属性规定了此 SVG 文件是否是“独立的”，或含有对外部文件的引用。-->
> <?xml version="1.0" standalone="no"?>
> <!--引用了外部的 SVG DTD。位于 “http://www.w3.org/.../svg11.dtd”。-->
> <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
> <!--
> SVG 代码以 <svg> 元素开始。
> width 和 height 属性可设置此 SVG 文档的宽度和高度。
> version 属性可定义所使用的 SVG 版本。
> xmlns 属性可定义 SVG 命名空间。
> SVG 的 <circle> 用来创建一个圆。cx 和 cy 属性定义圆中心的 x 和 y 坐标。如果忽略这两个属性，那么圆点会被设置为 (0, 0)。r 属性定义圆的半径。
> stroke 和 stroke-width 属性控制如何显示形状的轮廓。
> fill 属性设置形状内的颜色。
> -->
> <svg width="100%" height="100%" version="1.1" xmlns="http://www.w3.org/2000/svg">
>     <circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red"/>
> </svg>
> <!--
> 注：文档类型定义（DTD，Document Type Definition）是一种特殊文档，它规定、约束符合标准通用标示语言（SGML）或SGML子集可扩展标示语言（XML）规则的定义和陈述。
> -->
> ```

SVG 文件可通过以下标签嵌入 HTML 文档：embed、object 或者 iframe。

| 标签   | 目前情况                                                     |
| ------ | ------------------------------------------------------------ |
| embed  | 如果需要创建合法的 XHTML，就不能使用 <embed>。任何 HTML 规范中都没有 <embed> 标签 |
| object | 最新版本中，<object> 的可兼容性并不好                        |
| iframe | <iframe> 标签可工作在大部分的浏览器中。                      |

### 基本图形

SVG 有一些预定义的形状元素，可被开发者使用和操作：

- 矩形 <rect>
- 圆形 <circle>
- 椭圆 <ellipse>
- 线 <line>
- 折线 <polyline>
- 多边形 <polygon>
- 路径 <path>

#### rect 矩形

- rect 元素的 width 和 height 属性可定义矩形的高度和宽度
- x 属性定义矩形的左侧位置（例如，x="0" 定义矩形到浏览器窗口左侧的距离是 0px）
- y 属性定义矩形的顶端位置（例如，y="0" 定义矩形到浏览器窗口顶端的距离是 0px）
- rx 和 ry 属性可使矩形产生圆角

#### circle 圆形

- cx 和 cy 属性定义圆点的 x 和 y 坐标。如果省略 cx 和 cy，圆的中心会被设置为 (0, 0)
- r 属性定义圆的半径。

#### ellipse 椭圆

椭圆与圆很相似。不同之处在于椭圆有不同的 x 和 y 半径，而圆的 x 和 y 半径是相同的。

- cx 属性定义圆点的 x 坐标
- cy 属性定义圆点的 y 坐标
- rx 属性定义水平半径
- ry 属性定义垂直半径

#### line 线

- x1 属性在 x 轴定义线条的开始
- y1 属性在 y 轴定义线条的开始
- x2 属性在 x 轴定义线条的结束
- y2 属性在 y 轴定义线条的结束

#### polyline 折线

<polyline> 标签用来创建仅包含直线的形状。

- points 属性定义多边形每个角的 x 和 y 坐标

####  polygon 多边形

<polygon> 标签用来创建含有不少于三个边的图形。

- points 属性定义多边形每个角的 x 和 y 坐标

#### path 路径

下面的命令可用于路径数据：

- M = moveto
- L = lineto
- H = horizontal lineto
- V = vertical lineto
- C = curveto
- S = smooth curveto
- Q = quadratic Belzier curve
- T = smooth quadratic Belzier curveto
- A = elliptical Arc
- Z = closepath

**注释：**以上所有命令均允许小写字母。大写表示绝对定位，小写表示相对定位。

#### 部分样式

- CSS 的 fill 属性定义矩形的填充颜色（rgb 值、颜色名或者十六进制值）
- CSS 的 fill-opacity 属性定义填充颜色透明度（合法的范围是：0 - 1）
- CSS 的 stroke 属性定义矩形边框的颜色
- CSS 的 stroke-width 属性定义矩形边框的宽度
- CSS 的 stroke-opacity 属性定义笔触颜色的透明度（合法的范围是：0 - 1）

#### SVG 滤镜

##### 滤镜

SVG 滤镜用来向形状和文本添加特殊的效果，必须在 **<defs>** 标签中定义 SVG 滤镜。可用的滤镜有：

在 SVG 中，可用的滤镜有：

- feBlend
- feColorMatrix
- feComponentTransfer
- feComposite
- feConvolveMatrix
- feDiffuseLighting
- feDisplacementMap
- feFlood
- feGaussianBlur
- feImage
- feMerge
- feMorphology
- feOffset
- feSpecularLighting
- feTile
- feTurbulence
- feDistantLight
- fePointLight
- feSpotLight

##### Gaussian Blur 高斯模糊

<filter> 标签用来定义 SVG 滤镜。<filter> 标签使用必需的 id 属性来定义向图形应用哪个滤镜

- id 属性可为滤镜定义一个唯一的名称（同一滤镜可被文档中的多个元素使用）
- filter:url 属性用来把元素链接到滤镜。当链接滤镜 id 时，必须使用 # 字符
- 滤镜效果是通过 <feGaussianBlur> 标签进行定义的。fe 后缀可用于所有的滤镜
- <feGaussianBlur> 标签的 stdDeviation 属性可定义模糊的程度
- in="SourceGraphic" 这个部分定义了由整个图像创建效果

#### SVG 渐变

渐变是一种从一种颜色到另一种颜色的平滑过渡。可以把多个颜色的过渡应用到同一个元素上。

SVG 渐变必须在 **<defs>** 标签中进行定义。

在 SVG 中，有两种主要的渐变类型：

- 线性渐变
- 放射性渐变

##### linearGradient 线性渐变

线性渐变可被定义为水平、垂直或角形的渐变：

<linearGradient> 标签的 x1、x2、y1、y2 属性可定义渐变的开始和结束位置

- 当 y1 和 y2 相等，而 x1 和 x2 不同时，可创建水平渐变
- 当 x1 和 x2 相等，而 y1 和 y2 不同时，可创建垂直渐变
- 当 x1 和 x2 不同，且 y1 和 y2 不同时，可创建角形渐变

- id 属性可为渐变定义一个唯一的名称
- fill:url(#orange_red) 属性把 ellipse 元素链接到此渐变
- 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

##### radialGradient 放射渐变

- id 属性可为渐变定义一个唯一的名称
- fill:url(#grey_blue) 属性把 ellipse 元素链接到此渐变
- cx、cy 和 r 属性定义外圈，而 fx 和 fy 定义内圈
- 渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 <stop> 标签来规定。offset 属性用来定义渐变的开始和结束位置。

### [ SVG 实例 ](https://www.w3school.com.cn/svg/svg_examples.asp)

