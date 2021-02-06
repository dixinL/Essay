# Layui Table 组件的前端分页

##### [Essay](https://dixinl.github.io/Essay/)

## 起因

Layui Table 组件无法对大量数据进行前端分页由于使用了 github.io，暂时无法动态获取数据，且对于页面美观的要求，需要保留分页功能，数据部分的解决办法为 json 文件。但 json 文件无法处理数据信息，只能返回所有数据。然而前端 Layui 组件不支持前端分页。

## 条件

- limit 固定
- 对 url 的显示没有特殊要求

## 方法

​	使用 Table **page** 参数中的 **hash** 方法，获取 table 当前页

![1570618263689](./images/1570618263689.png)

​	在  **parseData** 方法中获取 url，并对 url 操作获取 page

![1570618326581](./images/1570618326581.png)

​	改变输出的 data 的值即可

![1570618227230](./images/1570618227230.png)