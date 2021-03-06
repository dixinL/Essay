# 机器学习-LeetCode

##### [Essay](https://dixinl.github.io/Essay/)

## 一、机器学习模型

机器学习算法与非机器学习算法（如控制交通灯的程序）的不同之处在于，它能够使自身的行为适应新的输入。而这种似乎没有人为干预的适应，偶尔会给人一种机器真的是在学习的错觉。

机器学习可以理解为一种函数，通过学习的输入数据不同得到的输出算法也不同。

## 二、有监督 VS 无监督

### 有监督学习

------

在一个***有监督***的学习任务中，数据样本将包含一个目标属性 y*y*，也就是所谓的***真值（ground truth）***。我们的任务是通过学习得到一个函数 F，它接受非目标属性 X，并输出一个接近目标属性的值，即 F(X) \approx y*F*(*X*)≈*y*。目标属性 y*y* 就像指导学习任务的教师，因为它提供了一个关于学习结果的基准。所以，这项任务被称为有监督学习。

在 Iris 数据集中，类别属性（鸢尾花的类别）可以作为目标属性。具有目标属性的数据通常称为 “***标记***” 数据（labeled data）。基于上述定义，，可以看出用标记数据预测鸢尾花的种类的任务是一个有监督的学习任务。

 

### 无监督学习

------

与有监督的学习任务相反，我们在***无监督***的学习任务中没有设置真值。人们期望从数据中学习潜在的模式或规则，而不以预先定义的真值作为基准。

人们可能会问，如果没有来自真值的*监督*，我们还能学到什么吗？答案是肯定的。以下是一些无监督学习任务的示例：

- **聚类（Clustering）**：给定一个数据集，可以根据数据集中样本之间的相似性，将样本聚集成组。例如，样本可以是一个客户档案，具有诸如客户购买的商品数量、客户在购物网站上花费的时间等属性。根据这些属性的相似性，可以将客户档案分组。对于聚集的群体，可以针对每个群体设计特定的商业活动，这可能有助于吸引和留住客户。

- **关联（Association）**：给定一个数据集，关联任务是发现样本属性之间隐藏的关联模式。例如，样本可以是客户的购物车，其中样本的每个属性都是商品。通过查看购物车，人们可能会发现，买啤酒的顾客通常也会买尿布，也就是说，购物车里的啤酒和尿布之间有很强的联系。有了这种学习而来的洞察力，超市可以将那些紧密相关的商品重新排列到相邻近的角落，以促进这一种或那一种商品的销售。

 

### 半监督学习

------

在数据集很大，但标记样本很少的情况下，可以找到同时具备有监督和无监督学习的应用。我们可以将这样的任务称为***半监督学习（semi-supervised learning******）***。

在许多情况下，收集大量标记的数据是非常耗时和昂贵的，这通常需要人工进行操作。但如果通过将有监督和无监督的学习结合在一个只有少量标记的数据集中，人们可以更好地利用数据集，并获得比单独应用它们更好的结果。

例如，人们想要预测图像的分类，但只对图像的 10% 进行了标记。通过有监督的学习，我们用有标记的数据训练一个模型，然后用该模型来预测未标记的数据，但是我们很难相信这个模型是足够普遍的，毕竟我们只用少量的数据就完成了学习。一种更好的策略是首先将图像聚类成组（无监督学习），然后对每个组分别应用有监督的学习算法。第一阶段的无监督学习可以帮助我们缩小学习的范围，第二阶段的有监督学习可以获得更好的精度。

## 三、分类 VS 回归

通常我们会根据输出值的类型将机器学习模型进一步划分为分类（classification）和回归（regression）。

> 如果机器学习模型的输出是离散值（discrete values），例如布尔值，那么我们将其称为分类模型。
>
> 如果输出是连续值（continuous values），那么我们将其称为回归模型。

### 问题转化

------

对于一个现实世界的问题，有时人们可以很容易地将其表述出来，并快速地将其归结为一个分类问题或回归问题。然而，有时这两个模型之间的边界并不清晰，人们可以将分类问题转化为回归问题，反之亦然。

对于照片识别模型，我们也可以将其从分类问题转换为回归问题。我们可以定义一个模型来给出一个介于 0~100% 之间概率值来判断照片中是否有猫，而不是给出一个二进制值作为输出。这样，就可以比较两个模型之间的细微差别，并进一步调整模型。例如，对于有猫的照片，模型 A 给出概率为 1%，而模型 B 对相同照片给出了 49% 的概率。虽然这两种模型都没有给出正确的答案，但我们可以看出，模型 B 更接近于事实。在这种情况下，人们经常应用一种称为逻辑回归（Logistic Regression）的机器学习模型，这种模型将连续概率值作为输出，但用于解决分类问题。