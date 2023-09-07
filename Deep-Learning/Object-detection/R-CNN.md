# R-CNN（2014）

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230414205056080.png" alt="image-20230414205056080" style="zoom: 33%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417165830439.png" alt="image-20230417165830439" style="zoom: 33%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416114328799.png" alt="image-20230416114328799" style="zoom: 45%;" />



候选区域搜索算法：Selective Search SS 选择性搜索

网络提取特征：AlexNet结构

SVM分类：共20个SVM，每一个SVM对于全部的2000个候选区域进行判断，是类别还是背景，得出[2000, 20]的得分矩阵

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230416115354155.png" alt="image-20230416115354155" style="zoom:50%;" />

非最大抑制（NMS）

目的：筛选候选区域，得到候选区域的最终区域结果

步骤：

1）根据概率值筛选概率值较大的候选框

2）找到最大的概率值候选框，剩余的候选框与之计算交并比，将IOU>阈值的剔除，重复上述操作

修正候选区域（bbox regressor）

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230414210317112.png" alt="image-20230414210317112" style="zoom: 25%;" />

训练过程：

1）正负样本划分

2000个候选区域与gt的IOU>0.5的样本构成正样本，IOU<0.5的样本构成负样本

2）神经网络训练

将标记好的正负样本送入在ImageNet上预训练的AlexNet网络进行训练微调

3）SVM训练

将正负样本通过神经网络后的输出特征作为SVM的输入，每个类别训练1个SVM

4）bbox训练

对于与ground truth的IOU超过阈值的候选区域进行回归，根据候选区域输出偏移量（感觉是在解决机器视觉与人眼视觉的矛盾）



为什么用SVM？

如果用全连接层会导致无法添加新的类别 



缺点：

1）训练阶段过多

2）训练占用空间大

3）处理速度慢

4）图片会产生形变

### 框架图



<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230414210121459.png" alt="image-20230414210121459" style="zoom: 25%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230414210809808.png" alt="image-20230414210809808" style="zoom: 25%;" />

