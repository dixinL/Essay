# DFS 和 BFS

##### [Essay](https://dixinl.github.io/Essay/)

## DFS 深度优先遍历

从图的某个顶点出发，**访问图中的所有顶点**，**且使每个顶点仅被访问一次**。这一过程叫做图的遍历。

**深度优先搜索的思想：**

1. 访问顶点 v；
2. 依次从 v 的未被访问的邻接点出发，对图进行深度优先遍历；直至图中和 v 有路径相通的顶点都被访问；
3. 若此时图中尚有顶点未被访问，则从一个未被访问的顶点出发，重新进行深度优先遍历，直到图中所有顶点均被访问过为止。

![991470-20170101222444523-572433338](./images/991470-20170101222444523-572433338.png)

在这里为了区分已经访问过的节点和没有访问过的节点，我们引入一个一维数组 bool visited[MaxVnum] 用来表示与下标对应的顶点是否被访问过。

**流程：**

1. 首先输出 V1，标记 V1 的 flag=true。
2. 获得 V1 的邻接边 [V2 V3]，取出V2，标记V2的flag=true。
3. 获得 V2 的邻接边 [V1 V4 V5]，过滤掉已经flag的，取出V4，标记V4的flag=true。
4. 获得 V4 的邻接边 [V2 V8]，过滤掉已经flag的，取出V8，标记V8的flag=true。
5. 获得 V8 的邻接边 [V4 V5]，过滤掉已经flag的，取出V5，标记V5的flag=true。
6. 此时发现V5的所有邻接边都已经被flag了，所以需要回溯。（左边黑色虚线，回溯到V1，回溯就是下层递归结束往回返）
    ![img](./images/991470-20170101222818336-1850167987.png)

**回溯：**

1. 回溯到 V1，在前面取出的是V2，现在取出V3，标记V3的flag=true。
2. 获得 V3 的邻接边 [V1 V6 V7]，过滤掉已经flag的，取出 V6，标记 V6 的 flag=true。
3. 获得 V6 的邻接边 [V3 V7]，过滤掉已经 flag 的，取出 V7，标记 V7 的 flag=true。
4. 此时发现 V7 的所有邻接边都已经被 flag 了，所以需要回溯。（右边黑色虚线，回溯到 V1，回溯就是下层递归结束往回返）

### 算法复杂度

数组表示：查找所有顶点的所有邻接点所需时间为 O(n2)，n 为顶点数，算法时间复杂度为 O(n2) 

## BFS 广度优先遍历

所谓广度，就是**一层一层的，向下遍历，层层堵截**，还是这幅图，我们如果要是广度优先遍历的话，我们的结果是 V1 V2 V3 V4 V5 V6 V7 V8。

　　　　　　![img](./images/991470-20170101221517351-1044059090.png)

**广度优先搜索的思想：**

1. 访问顶点 vi。
2. 访问 vi 的所有未被访问的邻接点 w1 ,w2 , …wk。
3. 依次从这些邻接点（在步骤 2 中访问的顶点）出发，访问它们的所有未被访问的邻接点；依此类推，直到图中所有访问过的顶点的邻接点都被访问。

　　　　　　　　 ② 访问vi 的所有未被访问的邻接点w1 ,w2 , …wk ；

　　　　　　　　 ③ 依次从这些邻接点（在步骤②中访问的顶点）出发，访问它们的所有未被访问的邻接点; 依此类推，直到图中所有访问过的顶点的邻接点都被访问；

> **说明：**
>
> 为实现 3，需要保存在步骤 2 中访问的顶点，而且访问这些顶点的邻接点的顺序为：先保存的顶点，其邻接点先被访问。 这里我们就想到了用标准模板库中的 queue 队列来实现这种先进现出的服务。

**流程：**

> **说明：**　
>
> 1. 将 V1 加入队列，取出 V1，并标记为 true（即已经访问），将其邻接点加进入队列，则 <—[V2 V3]
> 2. 取出 V2，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[V3 V4 V5]
> 3. 取出 V3，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[V4 V5 V6 V7]
> 4. 取出 V4，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[V5 V6 V7 V8]
> 5. 取出 V5，并标记为 true（即已经访问），因为其邻接点已经加入队列，则 <—[V6 V7 V8]
> 6. 取出 V6，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[V7 V8]
> 7. 取出 V7，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[V8]
> 8. 取出 V8，并标记为 true（即已经访问），将其未访问过的邻接点加进入队列，则 <—[]

###  算法复杂度

数组表示：查找每个顶点的邻接点所需时间为 O(n2)，n 为顶点数，算法的时间复杂度为 O(n2)