# DDPM

![image-20230313120522875](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230313120522875.png)

$x_0\rightarrow x_N$是一个添加噪音的过程

希望每一步所添加的噪音越来越多，$\beta_t=1-\alpha_t$，代表所增加的噪声的强弱，$z$是一个服从高斯分布的噪声，$z \sim N(0,1)$
$$
x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}z_1
$$
考虑$t-1$时刻：
$$
x_{t-1}=\sqrt{\alpha_{t-1}}x_{t-2}+\sqrt{1-\alpha_{t-1}}z_2
$$
代入上式：
$$
x_t=\sqrt{\alpha_t}(\sqrt{\alpha_{t-1}}x_{t-1}+\sqrt{1-\alpha_{t-1}}z_2)+\sqrt{1-\alpha_t}z_1
$$

$$
=\sqrt{\alpha_t\alpha_{t-1}}x_{t-1}+\sqrt{\alpha_t(1-\alpha_{t-1})}z_2+\sqrt{1-\alpha_t}z_1
$$

由于：
$$
\sqrt{\alpha_t(1-\alpha_{t-1})}z_2+\sqrt{1-\alpha_t}z_1\sim N(0,1-\alpha_t \alpha_{t-1})
$$
因此：
$$
x_t=\sqrt{\alpha_t\alpha_{t-1}}x_{t-2}+\sqrt{1-\alpha_t \alpha_{t-1}}\overline{z_1}
$$

$$
x_t=\sqrt{\overline{\alpha_t}}x_{0}+\sqrt{1-\overline{\alpha_t}}z_t
$$

但是我们主要希望的是如何求解逆向的过程，即
$$
q(x_{t-1}|x_t)=q(x_t|x_{t-1})\frac{q(x_{t-1})}{q(x_t)}
$$
其中$q$代表的是相应的概率分布

为了方便计算，引入$x_0$
$$
q(x_{t-1}|x_t,x_0)=q(x_t|x_{t-1},x_0)\frac{q(x_{t-1}|x_0)}{q(x_t|x_0)}
$$
由之前的推导可知
$$
q(x_t|x_{t-1},x_0)= \sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}z_1  \sim N(\sqrt{\alpha_t}x_{t-1},1-\alpha_t)
$$

$$
q(x_{t-1}|x_0)=\sqrt{\overline{\alpha_{t-1}}}x_{0}+\sqrt{1-\overline{\alpha_{t-1}}}z_{t-1} \sim N(\overline{\alpha_{t-1}}x_{0},1-\overline{\alpha_{t-1}})
$$

$$
q(x_{t}|x_0)=\sqrt{\overline{\alpha_{t}}}x_{0}+\sqrt{1-\overline{\alpha_{t}}}z_{t} \sim N(\overline{\alpha_{t}}x_{0},1-\overline{\alpha_{t}})
$$

因此，$q(x_{t}|x_0)$服从如下的分布：
$$
q(x_{t-1}|x_t,x_0)\varpropto \exp(-\frac{1}{2}(\frac{(x_t-\sqrt{\alpha_t}x_{t-1})^2}{1-\alpha_t}+\frac{(x_t-\overline{\alpha_{t-1}}x_{0})^2}{1-\overline{\alpha_{t-1}}}-\frac{(x_t-\overline{\alpha_{t}}x_{0})^2}{1-\overline{\alpha_{t}}}))
$$
经计算
$$
\hat{\mu_{t-1}}=\frac{1}{\sqrt{\alpha_t}}(x_t-\frac{\beta_t}{\sqrt{1-\overline{\alpha_t}}}\overline{z_t})
$$


（其中$x_0$被$x_t$替换掉了）

现在唯一的问题是$\overline{z_t}$不知道，因此希望能够通过一个模型根据$x_t$来预测$\overline{z_t}$

因此需要训练一个网络来预测$\overline{z_t}$，因此，正向添加噪声的过程可以看作是给图像添加标签的一个过程

![image-20230313165342472](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230313165342472.png)


$$
q(X)=\int q(X,Z)dZ=\int \frac{q(X,Z)p(Z|X)}{p(Z|X)}dZ
$$
![image-20230317121430499](https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317121430499.png)

证明：
$$
E[-\log p_{\theta}(x_0)]=\int-q(x_0,x_1,...,x_n)\log p_{\theta}(x_0)dx_0dx_1...dx_n=-\int q(x_0)\log p_{\theta}(x_0)dx_0
$$

$$
=-\int q(x_0)\log [\int p_{\theta}(x_{0:T})dx_{1:T}]dx_0=-\int q(x_0)\log [\int\frac{ p_{\theta}(x_{0:T})q(x_{1:T}|x_0)}{q(x_{1:T}|x_0)}dx_{1:T}]dx_0
$$

$$
\leq-\int q(x_0)q(x_{1:T}|x_0)\int\log [\frac{ p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}]dx_{1:T}dx_0=E_q[-\log \frac{ p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}]
$$

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317170546038.png" alt="image-20230317170546038" style="zoom:45%;" />

