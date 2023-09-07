# SPPNet（空间金字塔池化）(2015)

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416161309897.png" alt="image-20230416161309897" style="zoom: 25%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417170312890.png" alt="image-20230417170312890" style="zoom:33%;" />

解决R-CNN的问题：速度慢，图片变形

改进

速度慢：一张图片直接进行卷积运算

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416153220049.png" alt="image-20230416153220049" style="zoom: 25%;" />

步骤

1）把全图塞进CNN得到feature map

2）候选区域与feature map 直接映射，得到候选区域的特征向量（候选区域由SS得到）

3）映射后的特征向量大小不固定，这些特征向量塞给SPP层





候选区域映射

<img src="C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/image-20230416154129291.png" alt="image-20230416154129291" style="zoom: 50%;" />

左上角的点：

$x'=[\frac{1}{S}]+1$

右下角的点

$x'=[\frac{1}{S}]-1$

$S$是全部的stride的乘积



SPP层接受任何大小的输入，输出固定的特征维度

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416161048518.png" alt="image-20230416161048518" style="zoom: 67%;" />

缺点：训练时间长，训练阶段多，存储空间消耗大