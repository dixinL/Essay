# 解决 Github 中页面无法预览

##### [Essay](https://dixinl.github.io/Essay/)

## 问题复现

在写 echarts 分组篇时希望有预览功能，Markdown 写法如下

![1570528035787](./images/1570528035787.png)

实际结果：

![1570527997461](./images/1570527997461.png)

页面404

## 解决

- 写绝对路径

  ```
  https://dixinl.github.io/Essay/example
  ```

- 同时路径中不加分支信息

  - ![1570528128799](./images/1570528128799.png)
  - ![1570529594846](./images/1570529594846.png)

## 其他问题

页面可以显示，但会提示页面不安全，这是由于GitHub安全新机制

- 开发者引入不安全的库时会出现安全告警

### 解决办法

- js 代码尽量合并
- 如果可以引用，尽量不用本地库，使用https引用