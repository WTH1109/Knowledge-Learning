# Fast R-CNN(2015)

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416195649481.png" alt="image-20230416195649481" style="zoom:50%;" />



<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417170542854.png" alt="image-20230417170542854" style="zoom:33%;" />

步骤

1）图像送入卷积神经网络，得到整张图片的feature map

2）将region proposal（ROI）映射到feature map中

3）ROI pooling layer提取一个固定长度的特征向量，每个特征向量送入一系列全连接层，得到一个ROI特征向量

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416165119912.png" alt="image-20230416165119912" style="zoom:50%;" />

ROI pooling：

RoI pooling是一个简化的金字塔结构，目的是为了减少计算时间

原来SPP是4 * 4，2 * 2， 1 * 1

改为4 * 4

RoI池化将任何有效的RoI区域内的特征转化为具有H×W固定空间范围的小feature map



如果RoI区域过小怎么办？

比如RoI区域只有6×6，但是RoI pooling是7×7

动态调整，长6/7，宽6/7，进行取整



为什么用RoI pooling而不是SPP？

SPP相较于RoI pooling精度略高，但是RoI pooling的速度更快

SPP无法进行反向传播，因为金字塔池化会导致某一个点的梯度回传对应多个点，无法统一网络进行训练



### 多任务损失-Multi-task-loss

分类loss，N+1路的softmax输出，N是类别数，1是背景，采用交叉熵

回归loss，4×N的regressor，每个类别会训练一个回归器，用MAE损失



缺陷：Selective search耗死长，没有实现真正的端到端



## R-CNN,SPPNet,Fast R-CNN对比

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416195550991.png" alt="image-20230416195550991" style="zoom: 67%;" />

