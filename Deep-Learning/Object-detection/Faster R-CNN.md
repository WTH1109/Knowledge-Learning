# Faster R-CNN

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416201108787.png" alt="image-20230416201108787" style="zoom: 50%;" />

Fast R-CNN缺陷：选择性搜索速度慢

改进：将Fast R-CNN的选择性搜索产生候选框更改为利用RPN网络产生候选框

RPN网络产生每一个候选框的概率值以及坐标偏移量，训练时利用gt计算IoU根据阈值构建正负样本

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417171021693.png" alt="image-20230417171021693" style="zoom:33%;" />

RPN+Fast R-CNN

RPN：区域生成网络 	

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416211124646.png" alt="image-20230416211124646" style="zoom:50%;" />



步骤

1）输入任意大小的图像，经过CNN网络后输出特征图，特征图共享于后面两步

2）特征图通过RPN生成候选网络区域

3）候选区域和特征图共同输入到RoI pooling，得到每个候选区域的特征图，然后进行softmax分类和bbox预测



**RPN网络**

1）候选区域的产生：原图的每个像素点产生9个anchor boxes，长宽通过尺度和长宽比进行一一计算，得到最原始的anchor boxes

2）候选区域选择：每个ground-truth最高的Iou，与ground-truth的Iou>0.7标记为正样本，Iou<0.3标记为负样本，剩下的忽略

3）RPN是二分类，区别物体和背景

4）RPN regression：修正Bboxes的框的坐标



**RPN详细描述**

feature通过一个3×3的卷积层，进一步增大当下特征子点的感受野，得到H×W×256的feature map

注意，这里的3×3卷积相当于一个滑动窗口，每一个卷积就代表了一族anchor的中心点

feature map之后通过1×1的卷积，这个1×1的卷积本质上是在对每一个滑动窗口进行全连接，得到H×W×2k个分类特征，H×W×4k个偏移量

模型训练：

对于每张图片，每个ground-truth最高的Iou，与ground-truth的Iou>0.7标记为正样本，Iou<0.3标记为负样本，剩下的忽略，根据如下损失函数进行优化

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416222924584.png" alt="image-20230416222924584" style="zoom: 33%;" />



![image-20230416211040462](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416211040462.png)





缺陷：

训练参数过大，训练耗时大



如何改进？

YOLO删去RPN，直接对proposal进行分类回归
