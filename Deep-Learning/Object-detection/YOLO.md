# YOLO

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417112302240.png" alt="image-20230417112302240" style="zoom: 33%;" />

步骤

1）构建一个卷积神经网络

2）将图像传入卷积神经网络，获取卷积网络最终的特征层

3）最终层的每一个像素对应于原图的一片区域，比如最终的网络层是7×7×30的结构，那么就对应于将原图划分为7×7个patch

4）每一个patch最终输出两个bbox，具体信息包括坐标值，预测每一类的概率值，以及置信度，比如对于一个20类的检测问题，每一个patch最终输出的值为20+4+1+4+1=30

5）最终图像输出49×2个bbox



训练阶段

- 每个bbox对应一个置信度，如果bbox中无object，则置信度为0，否则为bbox与ground truth的IoU（感觉就是RPN网络的概率值）
- 如何判断bbox中是否含有object？如果object的gt的中心点在bbox中，则称bbox包含这个object
- 每个单元格输出一个confidence高的bbox位置，以及一个概率较大的类别

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230417121757841.png" alt="image-20230417121757841" style="zoom: 50%;" />



bbox位置（x,y,w,h）

（x，y）：在当前单元格内的中心偏移量

（w，h）：相对于原始长宽的长宽比





缺陷思考：

需要有标注的bbox框，而bbox框的标注纯粹依赖于人的手动标注

同时检测阶段是通过类似查找的方式，机器仍不能够自动找到检测框



优点：

速度快

缺点：

准确率欠缺，对于相互靠近的物体识别率一般







