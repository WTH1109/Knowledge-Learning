# K近邻算法

思路：取离目标点最近的k个点，里面最多的类别即为目标点所分的类

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230410121908151.png" alt="image-20230410121908151" style="zoom:67%;" />

正常而言需要比对目标点与空间中的全部点，从而得到最近邻，是否有办法快速查找呢？

## 快速搜索算法

### KD-Tree

划分思想：按照中位数进行划分，本质是利用超平面将数据划分为树结构

步骤1：构造KD-Tree

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230410123642735.png" alt="image-20230410123642735" style="zoom:50%;" />

步骤2：搜索目标点对应的叶节点，把该节点当作暂时的最邻近点

步骤3：进行回溯，如果存在节点距离比叶节点更近，则把新的节点作为最邻近节点

步骤4：如果当前节点有分支，以目标点和目前的最邻近点为半径作圆，进入与圆相交的子空间进行查找

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230410125032165.png" alt="image-20230410125032165" style="zoom: 50%;" />



### Ball-Tree

树的构建

1）找到样本中心点

2）找到距离中心点最远的两个点m1，m2

3）将数据按照对于m1，m2的距离进行划分

4）重复上述过程



### 改进算法

**剪辑近邻法**

步骤

1）样本集随机划分为$S$个子集

2）用第$i+1$个样本集对于第$i$个样本集进行分类

3）去掉分类错误的样本

4）将剩余的样本构成新的样本集

5）回到步骤1，当没有样本被删除时停止



**压缩临近法**

基本思想：

1）用现有样本重新生成一个样本集

2）新的样本集要求保留最少的样本量，同时能够对原有样本用最邻近法进行分类，可以保证模型的泛化能力

步骤

1）定义两个存储器，Store和Grabbag，Store存储即将生成的样本集，Grabbag存储原样本集

2）[初始化] 从Grabbag中取出任一个样本放入Store中

3）[生成样本集] 用Store中的样本对于Grabbag中的样本分类，若分类错误则将该样本放入Store中，否则扔回Grabbag中

4）[终止条件] 当所有样本执行时没有转入Store的现象，说明Store中的全部样本构建的模型已经可以对Grabbag进行分类，或者Grabbag变成空集



**最近质心法**

跟KMeans算法相同，用每一类的均值代表该类，分类样本距离哪个质心最近就分为哪类





## 回归任务

标签值用邻近点的均值代替

KNeighborsRegressor：固定点数

RadiusNeighborsRegressor：固定半径

距离权重：距离越近贡献越大



