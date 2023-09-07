# Grad-CAM

优势

（a）帮助找到失败的模型失败的原因

（b）在ILSVRC-15的弱监督定位任务中表现优于其他模型

（c）对于强扰动鲁棒性强

（d）更加专注底层原理

（e）通过确认数据的偏差来增加模型的泛化性

同时该方法可以帮助人们建立起对于网络的信任

良好的视觉解释：

（a）在图像中定位类别

（b）高分辨率

![image-20221111120422535](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221111120422535.png)

## 原理

Grad-CAM使用流入CNN最后一个卷积层的梯度信息来为每一个神经元分配一个重要性值，从而做出对于感兴趣区域的决定。

为了计算类别c的类别判定定位图，我们首先计算类别c的分数$y^c$关于卷积层的激活层$A_k$的导数$\frac{\partial y^c}{\partial A^k}$（$y^c$在softmax层之前.）除了期望判断的类之外的梯度值都设置为0，期望的值设置为1。（注意，Fig.2中的A指的是输出的特征层，而不是一个网络）

如Fig.2所示，A所指代的是在全连接层之前的最后一个卷积层的输出值，每一个神经元通过平均池化来得到反向传播时神经元的重要程度$\alpha_k^c$，代表A中每个层对于输出结果的贡献程度
$$
\alpha_k^c=\frac{1}{Z}\sum_{i}\sum_{j}\frac{\partial y^c}{\partial A_{ij}^k}
$$
在计算$\alpha_k^c$的同时对于激活的梯度进行反向传播，如果要精确的计算该值，就相当于去计算权重矩阵和关于激活函数的矩阵梯度的连续的乘积，直到梯度反向传播回最终的卷积层。
$$
Z=wX+b
$$

$$
A=\sigma(wX+b)
$$

将$\alpha_k^c$与$A$相乘即得到哪些特征对于输出影响较大
$$
L_{Grad-CAM}^c=ReLU(\sum_k \alpha_k^c A^k)
$$


## guided backpropagation

反卷积网络：

反卷积网络会将高层级的特征图反向传播回CNN的数据流，从给定层的神经激活元到图像。

通常来说，高层级的神经元是非零的，通过结果重建出的图片展示了输入图片中最强烈影响该神经元的部分

![image-20221111110929633](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221111110929633.png)

池化层：为了重建通常来说不可逆的最大池化层， Zeiler and Fergus的方法需要先进行一次正向传播去计算出开关，也就是每一个池化区域内最大值的位置。这些开关被用于反卷积方法

非线性层：反卷积与反向传播主要在Relu上的处理不同，反卷积反向时剔除掉为负的部分，反向传播剔除掉正向传播时为负的部分

反卷积相当于反向传播，除了在通过非线性传播时，其梯度仅根据顶部梯度计算，忽略了底部输入

guided backpropagation:将反卷积与反向传播所共同忽略掉的部分取0

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221111112207196.png" alt="image-20221111112207196" style="zoom:67%;" />