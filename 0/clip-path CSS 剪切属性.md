# clip-path CSS 剪切属性

##### [Essay](https://dixinl.github.io/Essay/)

clip-path CSS 属性可以创建一个只有元素的部分区域可以显示的剪切区域。区域内的部分显示，区域外的隐藏。剪切区域是被引用内嵌的URL定义的路径或者外部svg的路径，或者作为一个形状。clip-path属性代替了现在已经弃用的剪切 [clip](https://developer.mozilla.org/en-US/docs/Web/CSS/clip) 属性。

## 取值

clip-path: <clip-source> | [ <basic-shape> || <geometry-box> ] | none

***属性说明***
<clip-source> = <url>
<basic-shape> = <inset()> | <circle()> | <ellipse()> | <polygon()>
<geometry-box> = <shape-box> | fill-box | stroke-box | view-box

其默认值是none。

- <clip-source>

  用 <url> 表示剪切元素的路径

- <basic-shape>

  一种形状，其大小和位置由 <几何盒> 值定义。如果没有指定几何框，则边框将用作参考框

- <geometry-box>

  如果同 <basic-shape> 一起声明，它将为基本形状提供相应的参考框盒。通过自定义，它将利用确定的盒子边缘包括任何形状边角（比如说，被 border-radius 定义的剪切路径）。几何框盒可以有以下的值中的一个：

  - margin-box 作为引用框。

  - border-box 作为引用框。
  - padding-box 作为引用框。
  - content-box 作为引用框。
  - fill-box 利用对象边界框作为引用框。
  - stroke-box 使用笔触边界框（stroke bounding box）作为引用框。
  - view-box 使用最近的 SVG 视口（viewport）作为引用框。如果 viewBox 属性被指定来为元素创建 SVG 视口，引用框将会被定位在坐标系的原点，引用框位于由 viewBox 属性建立的坐标系的原点，引用框的尺寸用来设置 viewBox 属性的宽高值。

- none

  不创建的剪切路径。

使用clip-path有两点要注意：

1. 使用clip-path要从同一个方向绘制，如果顺时针绘制就一律顺时针，逆时针就一律逆时针，因为polygon是一个连续线段，若线段彼此有交集，裁剪区域就会有相减的情况发生，当然如果你特意需要这样的效果除外。
2. 如果绘制时采用比例的方式绘制，长宽就必须要先行设定，不然有可能绘制出来的长宽和我们想像的就会有差距，使用像素绘制就不会有这样的现象。

![894563958-57d3e2cf3429c_articlex](./images/894563958-57d3e2cf3429c_articlex.png)

## clip-path 实践网站

http://bennettfeely.com/clippy/

# 𓀀𓀁𓀂𓀃𓀄𓀉𓀊𓀋𓀌𓀍𓀏𓀒𓀓𓀔𓀕𓀖𓀜𓀝𓀞𓀟𓀠𓀡𓀢𓀣𓀤𓀥𓀦𓀧𓀨𓀩𓀻𓀼𓀽𓀾𓁁𓁂𓁃𓁄𓁅𓁆𓁇𓁈𓁉𓀀𓀃𓀉𓀊𓀋𓀌𓀍𓀎𓀏𓀓𓀔𓀕𓀖𓀞𓀟𓀠𓀡𓀢𓀣𓀤𓀥𓀦𓀧𓀨𓀾𓁁