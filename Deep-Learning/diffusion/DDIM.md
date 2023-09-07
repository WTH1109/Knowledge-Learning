# DDIM

## 背景回顾

$q(x_0)$是数据的真实分布，我们希望能够通过模型来学习分布$p_{\theta}(x_0)$来近似$q(x_0)$

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230314104343659.png" alt="image-20230314104343659" style="zoom:50%;" />

其中，$p_{\theta}(x_{0:T})$是$x_0,x_1,...,x_T$的联合分布

回顾DDPM，其损失函数为

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317202109183.png" alt="image-20230317202109183" style="zoom: 67%;" />

只需要满足以下要求
$$
q_{\sigma}(x_t|x_0)\sim \mathcal{N}(\sqrt{\alpha_t}x_0,(1-\alpha_t)I)
$$
因此只需要满足该条件不变就可以套用DDPM的训练过程

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317200918812.png" alt="image-20230317200918812" style="zoom:50%;" />

定义如上的几个公式，我们希望依旧能够满足$q_{\sigma}(x_t|x_0)\sim \mathcal{N}(\sqrt{\alpha_t}x_0,(1-\alpha_t)I)$

同时也可以发现，这也并没有要求前向过程必须为马尔可夫过程

但是，我们需要关注一个重点，即后验的扩散概率
$$
q(x_{t-1}|x_t,x_0)
$$
需要可以用公式表达，我们才可以对于网络进行建模（写出其均值，标准差）

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317200401944.png" alt="image-20230317200401944" style="zoom: 45%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317205638096.png" alt="image-20230317205638096" style="zoom: 45%;" />





根据

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317214014525.png" alt="image-20230317214014525" style="zoom: 60%;" />

以及
$$
q_{\sigma}(x_t|x_0)\sim \mathcal{N}(\sqrt{\alpha_t}x_0,(1-\alpha_t)I)
$$
得到

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317213842950.png" alt="image-20230317213842950" style="zoom:45%;" />

令$\sigma_t=0$，得到DDIM

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230423153848921.png" alt="image-20230423153848921" style="zoom:50%;" />