# Docker&K8S简述

##### [Essay](https://dixinl.github.io/Essay/)

## Docker

### 历史

- 10 年创建，dotCloud
- 后更名为docker
- 13年开源

### 与虚拟机的区别

![img](images/v2-c2a31e2008835b2974170ad1dbac0d42_hd.jpg)

### Docker概要

- **Docker本身并不是容器**，它是创建容器的工具，是应用容器引擎。

- **Build, Ship and Run**， ”搭建、发送、运行“。

- **Build once，Run anywhere**，搭建一次，到处能用。

  #### 核心

  - **镜像（Image）**
  - **容器（Container）**
  - **仓库（Repository）**

  不用的时候把镜像放在仓库里，需要用的时候使用镜像到容器里

  对于仓库，**Docker Registry服务**负责对镜像进行管理。最常使用的Registry公开服务，是官方的**Docker Hub**，这也是默认的 Registry，并拥有大量的高质量的官方镜像。

## K8S

Kubernetes 这个单词来自于希腊语，含义是**舵手**或**领航员**。K8S是它的缩写，用“8”字替代了“ubernete”这8个字符。

一个K8S系统，通常称为一个**K8S集群（Cluster）**。

这个集群主要包括两个部分：

- **一个Master节点（主节点）**
- **一群Node节点（计算节点）**

![img](images/v2-466804fc47bd2e939e0413d9c32170af_r.jpg)

### Master

![img](images/v2-7fa63b292368c8f21bd4582861a6983d_r.jpg)

Master节点包括API Server、Scheduler、Controller manager、etcd。

- API Server是整个系统的对外接口，供客户端和其它组件调用，相当于“营业厅”。
- Scheduler负责对集群内部的资源进行调度，相当于“调度室”。
- Controller manager负责管理控制器，相当于“大总管”。

### Node

![img](images/v2-8cb338cd8923fa0e6857f45facc8f00f_hd.jpg)  

Node节点包括**Docker、kubelet、kube-proxy、Fluentd、kube-dns（可选），还有就是Pod**。Pod是Kubernetes最基本的操作单元。一个Pod代表着集群中运行的一个进程，它内部封装了一个或多个紧密相关的容器。除了Pod之外，K8S还有一个**Service**的概念，一个Service可以看作一组提供相同服务的Pod的对外访问接口。这段不太好理解，跳过吧。

​	**Docker**，不用说了，创建容器的。

​	**Kubelet**，主要负责监视指派到它所在Node上的Pod，包括创建、修改、监控、删除等。

​	**Kube-proxy**，主要负责为Pod对象提供代理。

​	**Fluentd**，主要负责日志收集、存储与查询。

## 未来发展

### 核心网

从几十年前的1G，到现在的4G，再到将来的5G，移动通信发生了翻天覆地的变化，核心网亦是如此。但是，所谓的核心网，其实本质上并没有发生改变，无非就是很多的服务器而已。不同的核心网网元，就是不同的服务器，不同的计算节点。  变化的，是这些“服务器”的形态和接口：形态，从机柜单板，变成机柜刀片，从机柜刀片，变成X86通用刀片服务器；接口，从中继线缆，变成网线，从网线，变成光纤。**就算变来变去，还是服务器，是计算节点，是CPU**。

以VoLTE为例，如果按以前2G/3G的方式，那需要大量的专用设备，分别充当EPC和IMS的不同网元。

![img](images/v2-db0d325f60de323d1346d9d4e0eab1bd_hd.jpg)

而采用容器之后，很可能只需要一台服务器，创建十几个容器，**用不同的容器，来分别运行不同网元的服务程序**。

![img](images/v2-ebbe757bef45be1ecde8827bb2e1c0bc_hd.jpg)

这些容器，随时可以创建，也可以随时销毁。还**能够在不停机的情况下，随意变大，随意变小，随意变强，随意变弱，在性能和功耗之间动态平衡**。核心网采用微服务架构，也是和容器完美搭配——**单体式架构（Monolithic）**变成**微服务架构（Microservices）**，相当于一个全能型变成N个专能型。**每个专能型，分配给一个隔离的容器**，赋予了最大程度的灵活。

按照这样的发展趋势，在移动通信系统中，除了天线，剩下的部分都有可能虚拟化。核心网是第一个，但不是最后一个。**虚拟化之后的核心网，与其说属于通信，实际上更应该归为IT**。核心网的功能，只是容器中普通一个软件功能而已。

借用**Atwood’s Law**。Jeff Atwood在2007年提出的：“**any application that can be written in JavaScript, will eventually be written in JavaScript.**”。**任何可以被虚拟化的东西，都会被虚拟替代**。

###### 本文摘抄自 知乎
