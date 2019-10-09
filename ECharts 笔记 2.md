# ECharts 笔记 2

##### [Essay](https://dixinl.github.io/Essay/)

## 用户行为

在 ECharts 中事件分为两种类型，一种是用户鼠标操作点击，或者 hover 图表的图形时触发的事件，还有一种是用户在使用可以交互的组件后触发的行为事件，例如在切换图例开关时触发的 ['legendselectchanged'](https://echarts.baidu.com/api.html#events.legendselectchanged) 事件（这里需要注意切换图例开关是不会触发`'legendselected'`事件的），数据区域缩放时触发的 ['datazoom'](https://echarts.baidu.com/api.html#events.legendselectchanged) 事件等等。

- ECharts 支持常规的鼠标事件类型，包括

> click、dblclick、mousedown、mousemove、mouseup、mouseover、mouseout、globalout、contextmenu

- 所有的鼠标事件包含参数 `params`，这是一个包含点击图形的数据信息的对象，如下格式：

> {
>     // 当前点击的图形元素所属的组件名称，
>     // 其值如 'series'、'markLine'、'markPoint'、'timeLine' 等。
>     componentType: string,
>     // 系列类型。值可能为：'line'、'bar'、'pie' 等。当 componentType 为 'series' 时有意义。
>     seriesType: string,
>     // 系列在传入的 option.series 中的 index。当 componentType 为 'series' 时有意义。
>     seriesIndex: number,
>     // 系列名称。当 componentType 为 'series' 时有意义。
>     seriesName: string,
>     // 数据名，类目名
>     name: string,
>     // 数据在传入的 data 数组中的 index
>     dataIndex: number,
>     // 传入的原始数据项
>     data: Object,
>     // sankey、graph 等图表同时含有 nodeData 和 edgeData 两种 data，
>     // dataType 的值会是 'node' 或者 'edge'，表示当前点击在 node 还是 edge 上。
>     // 其他大部分图表中只有一种 data，dataType 无意义。
>     dataType: string,
>     // 传入的数据值
>     value: number|Array
>     // 数据图形的颜色。当 componentType 为 'series' 时有意义。
>     color: string
> }

在 ECharts 2.x 是通过 `myChart.component.tooltip.showTip` 这种形式调用相应的接口触发图表行为，入口很深，而且涉及到内部组件的组织。相对地，在 ECharts 3 里改为通过调用 `myChart.dispatchAction({ type: '' })` 触发图表行为，统一管理了所有动作，也可以方便地根据需要去记录用户的行为路径。

常用的动作和动作对应参数在 [action](https://echarts.baidu.com/api.html#action) 文档中有列出。

## 源码

- 源码使用webpack打包，查看文件webpack.config.js可知，将echarts源码编译成三个版本，分别为常用版本,精简版本，完整版本，分别对应webpack入口文件为index.common.js、index.simple.js、index.js。三个文件引用的都是lib文件下的文件，执行下面一步提示的命令npm insall后就可以得到lib文件夹，它里面的文件和src文件夹中的文件主要内容是相同的，不同之处在于：前者文件是通过类似CMD的模式打包的，后者文件是通过webpack进行打包的。

- **components和charts**：charts是指各种类型的图表，例如line，bar，pie等，在配置项中指的是series对应的配置；components组件是在配置项中除了series的其余项，例如title，legend，toobox等。

- 主要文件路径
  - extension （扩展中使用）
  - lib （源码中没有，执行webpack编译后才存在）
  - map （世界地图，中国地图及中国各个省份地图的js和json两种格式的文件）
  - src （核心源码）
  - test （示例demo）
  - theme （主题）

- echarts渲染了两个div，一个div用来渲染主要的图表的，div里面嵌套一个canvas标签，第二个div是为了显示hover层信息的。

## init方法

源码如下

> echarts.init = function(dom, theme, opts) {
>     if (\_\_DEV\_\_) {//是否是debug模式
>         //...     //错误判断这部分内容省略
>     }
>
> var chart = new ECharts(dom, theme, opts);//实例化ECharts
> chart.id = 'ec_' + idBase++;//chart实例的id号，唯一，逐一递增
> instances[chart.id] = chart;//唯一instance(实例)对象
>
> dom.setAttribute &&
>     dom.setAttribute(DOM_ATTRIBUTE_KEY, chart.id);//为外层dom设置了一个属性，属性值等于chart.id
>
> enableConnect(chart);//按照顺序更新状态，一共三个状态
>     /\*var STATUS_PENDING = 0;
>     var STATUS_UPDATING = 1;
>     var STATUS_UPDATED = 2;\*/
>
> return chart;
>
> };

主题theme，可以在官网下载(http://echarts.baidu.com/download-theme.html),或者自己构建
> 使用：
>
> `<script src="theme/vintage.js"></script>`
>
> `<script>`
>
> // 第二个参数可以指定前面引入的主题
>
> var chart = echarts.init(document.getElementById('main'), 'vintage');
>
> `</script>`







