# A Survey on Generative Diffusion Model

## Abstract：

- Diffusion缺陷：缓慢的生成进程、单一的数据类型、低对数似然、难以做到数据降维
- 两个标志性的工作：DDPM，DSM；统一的标志性工作：Score SDE
- 类别改进技术
- 模型加速技术：训练策略、采样策略、混合模型、分数和扩散统一模型
- 数据类型改进：连续空间、离散空间、约束空间
- 对数似然优化：改进ELBO并最小化变分间隙
- 数据降维技术
- 模型评估：FID、IS、NLL
- Diffusion模型应用：计算机视觉、序列模型、音频、AI for science
- 目前的局限性和未来的发展



## Introduction:

- 现有的生成模型：GAN（生成对抗网络）、VAE（变分自编码器，表征为潜变量）、EBM（能量先验模型）、normalizing flow （标准流，学习概率分布）、diffusion模型

  <img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230503213929139.png" alt="image-20230503213929139" style="zoom:50%;" />

  EBM模型：训练通过x和潜变量z去学习x的分布，通过最小化$\tilde{y}$与$x$之间的能量函数所实现。同时需要一个判别器去最大化$\tilde{y}$与其他样本$y$之间的能量函数。采样时无需输入图片$x$

- 为什么选择diffusion？VAE需要对齐的后验分布，EBM需要求解困难的分区函数、GAN需要额外训练判别器、normalizing flow 需要设置网络约束

  diffusion优点：稳定的训练流程、有效的理论支撑、统一的简洁损失函数设计

  缺点：大量的采样步骤需要大量的采样时间、对数似然优化、维度减少

- 改进的模型举例

  DPM-solver ：利用ODE的平稳性加速采样过程至10步之内

  D3PM：文本和分类融入混合损失中

- diffusion的改进方法：

  1）加速模型

  2）数据类型多样化

  3）对数似然优化

  4）维度减少

- 本文贡献：总结diffusion、提出四种改进方法、diffusion模型应用、模型限制



## PROBLEM STATEMENT

### Notions and Definitions

1.状态：$x_t$代表$t$时刻的状态

2.转换内核：$F(x,\sigma),R(x,\sigma)$

3.训练策略：最大化对数似然
$$
E_{F(x_0,\sigma)}[-\log R(x_T,\tilde{\sigma})]
$$

### Problem Formulation

**1.DDPM**

**前向过程**
$$
F_t(x_{t-1},\beta_t)=q(x_t|x_{t-1})=N(x_t;\sqrt{1-\beta_t}x_{t-1},\sqrt{\beta_t}I)
$$

$$
F(x_0,\beta)=q(x_{1:T}|x_0)=q(x_{1:T-1}|x_0)q(x_{T}|x_{0:T-1})=q(x_{1:T-1}|x_0)q(x_{T}|x_{T-1})=...=\prod_{t=1}^{T} q(x_t|x_{t-1})(Markov)
$$

**反向过程**
$$
R_t(x_t,\Sigma_\theta)=p_{\theta}(x_{t-1}|x_t)=N(x_{t-1};\mu_{\theta}(x_t,t),\Sigma_{\theta}(x_t,t))
$$

$$
p(x_T)=N(x_T;\bold{0},\bold{I})
$$

$$
R(x_T,\Sigma_{\theta})=p_{\theta}(x_{0:T})=p_{\theta}(x_{1:T})p_{\theta}(x_0|x_{1:T})=p_{\theta}(x_{1:T})p_{\theta}(x_0|x_1)=...=p(x_T)\prod_{t=1}^{T}p_{\theta}(x_{t-1}|x_t)
$$

（*）DDPM的前向与反向过程均为马尔可夫链

**训练目标**

最小化对数似然
$$
E[-\log p_{\theta}(x_0)]=\int-q(x_0,x_1,...,x_n)\log p_{\theta}(x_0)dx_0dx_1...dx_n=-\int q(x_0)\log p_{\theta}(x_0)dx_0
$$

$$
=-\int q(x_0)\log [\int p_{\theta}(x_{0:T})dx_{1:T}]dx_0=-\int q(x_0)\log [\int\frac{ p_{\theta}(x_{0:T})q(x_{1:T}|x_0)}{q(x_{1:T}|x_0)}dx_{1:T}]dx_0
$$

$$
\leq-\int q(x_0)q(x_{1:T}|x_0)\int\log [\frac{ p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}]dx_{1:T}dx_0=E_q[-\log \frac{ p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}]
$$

$$
E[-\log p_{\theta}(x_0)]\leq E_q[-\log \frac{ p_{\theta}(x_{0:T})}{q(x_{1:T}|x_0)}]=E_q[-\log p(x_T)-\sum_{t\geq1}\log \frac{p_{\theta} (x_{t-1}|x_t)}{q(x_t|x_{t-1})}]
$$

$$
=E_q[D_{KL}(q(x_T|x_0)||p(x_T	))]+\sum_{t>1}D_{KL}(q(x_{t-1}|x_t,x_0)||p_{\theta}(x_{t-1}|x_t))=L
$$

**2.DSM(Denoised Score Matching)**

**原理**

通过近似数据$\triangledown_x \log p(x)$的梯度，解决原始数据分布的估计问题。将数据的梯度称为分数，通过训练一个网络来生成，在训练时利用不同的噪声网络来获得分数。

由于**EBM**模型可以写为
$$
p_{\theta}(x)=\frac{1}{Z(\theta)}\exp[-E_{\theta}(x)]
$$
因此
$$
\triangledown_x \log p(x)=-\triangledown_xE_{\theta}(x)
$$
**分数扰动处理&扰动核**

高斯扰动核
$$
q_{\sigma}(\tilde{x}|x)=N(\tilde{x}|x,\sigma^2\bold{I})
$$
***引理：Tweedie Estimator***

若$p(x|\theta)=N(\theta,\sigma^2)$，则有
$$
\hat{\theta}=x+\sigma^2\frac{d}{dx}\log p(x)
$$
推导链接：[一文解释 经验贝叶斯估计, Tweedie's formula - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/594007789)

**3.Score SDE Formulation**

$$
dx=f(x,t)dt+g(t)dw
$$
解释：$x$的变化可以抽象为依赖于时间的变化和噪声的变化

比如**DDPM**的前向过程：
$$
x_t=\sqrt{\alpha_t}x_{t-1}+\sqrt{1-\alpha_t}z_{t}
$$

$$
x_t-x_{t-1}=(\sqrt{\alpha_t}-1)x_{t-1}+\sqrt{1-\alpha_t}z_{t}
$$

$$
x_t-x_{t-1}=-(1-\sqrt{\alpha_t})x_{t-1}+\sqrt{1-\alpha_t}z_{t}
$$

$$
dx/dt=-\frac{1-\alpha_t}{1+\sqrt{\alpha_t}}+\sqrt{\beta}dw/dt
$$

$1+\sqrt{\alpha_t}\approx2$

因此

$$
dx=-\frac{\beta(t)x}{2}dx+\sqrt{\beta(t)}dw
$$





