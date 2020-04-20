# Transfolw.js picture-train

##### [Essay](https://dixinl.github.io/Essay/)

*本篇文章用于 W3C-tfjs 图片训练中技术点解析*

## 训练方法

通过训练数据“查看”数以千计的数字图片以及他们对应的标识来训练分辨器。然后再通过此模型从未“见到”过的测试数据评估这个分辨器的精确度。

## 数据集

MNIST 数据集来自美国国家标准与技术研究所, National Institute of Standards and Technology (NIST)。训练集 (training set) 由来自 250 个不同人手写的数字构成, 其中 50% 是高中学生, 50% 来自人口普查局 (the Census Bureau) 的工作人员. 测试集(test set) 也是同样比例的手写数字数据.

可从http://yann.lecun.com/exdb/mnist/获得的 MNIST 手写数字数据库的训练集为 60,000 个示例，而测试集为 10,000 个示例。它是 NIST 可提供的更大集合的子集。这些数字已进行尺寸规格化，并在固定尺寸的图像中居中。

