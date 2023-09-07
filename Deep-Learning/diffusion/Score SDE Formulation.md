# Score SDE Formulation

## **SGM分数生成模型**-SMLD

### 分数函数

根据**能量模型（Energy-Based Models，EBM）**模型，任意一个数据分布可以写为
$$
p_{\theta}(x)=\frac{1}{Z(\theta)}\exp[-E_{\theta}(x)]
$$
在这里面，$Z(\theta)$是一个归一化常数，$Z(\theta)=\int \exp[-E_{\theta}(x)] dx$



为了**消除**$Z(\theta)$的影响，对式子两边同时求**梯度**，得到
$$
\triangledown_x \log p(x)=-\triangledown_xE_{\theta}(x)
$$
可以发现$\triangledown_x \log p(x)$与$Z(\theta)$无关。我们称$s_{\theta}(x)=\triangledown_x \log p(x)$为**分数函数**，只要能够找到正确的分数函数，我们就可以得到**空间中目标区域**的所在位置而无需计算$Z(\theta)$



那么，得到**分数函数**后该怎样找到$x$的**正确的分布**呢？——  **郎之万动力学采样**



### 朗之万动力学（Langevin dynamics）采样

***引理：Tweedie Estimator***

若$p(x|\theta)=N(\theta,\sigma^2)$，则有
$$
\hat{\theta}=x+\sigma^2\frac{d}{dx}\log p(x)
$$
推导链接：[一文解释 经验贝叶斯估计, Tweedie's formula - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/594007789)



可以看出，条件概率的均值估计与$x$以及$\triangledown_x \log p(x)$有关，因此可以通过迭代的方式去尽可能近似$x$的真实分布（个人理解）

**郎之万动力学**即这样的一种采样方式：
$$
x_{t+1}\leftarrow x_t+\delta \triangledown \log p(x_t) + \sqrt{2\delta}\varepsilon_t
$$
当$T$足够大时，$x_T$变为真实数据，具体的采样过程由下动画所示

<iframe src="https://vdn6.vzuu.com/SD/83349034-5927-11ed-a869-76a40522aa1e.mp4?pkey=AAWE3WnWfL7NJH06jrkHg4Hk4cM-sYqCU488pe6bwwudKHoYH5dLDhhfOvZJg_xc_X4oyPRNzB4i0vTzYwTcfaKv&c=avc.1.1&f=mp4&pu=078babd7&bu=078babd7&expiration=1683431539&v=ks6" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" height='320'> </iframe>



### 如何得到分数函数—Score matching

该问题可以用一个优化问题表示，我们通过训练一个网络来估计真实的**分数函数**$s_{\theta}(x)$：
$$
L=E_{p(x)}[\vert\vert{\triangledown_x \log p(x)-s_{\theta}(x)||^2}]
$$
**问题：$\triangledown_x \log p(x)$实际上并不知道，无法优化该问题**

**解决方法：Score-matching，无需知道真实的$p(x)$就可以最小化该loss**



**ISM**

**推导过程：**
$$
L=E_{p(x)}[\vert\vert{\triangledown_x \log p(x)-s_{\theta}(x)\vert\vert^2}]=\int p(x)[||{\triangledown_x \log p(x)}||^2+||s_{\theta}(x)||^2-2(\triangledown_x \log p(x))^Ts_{\theta}(x)]dx
$$
我们分开来看损失函数的三项

1.  $ p(x)[||{\triangledown_x \log p(x)}||^2$相对于$\theta$是常数项可忽略
2.  $\int p(x)||s_{\theta}(x)||^2dx=E_{p(x)}[||s_{\theta}(x)||^2]$
3. $-2\int p(x)(\triangledown_x \log p(x))^Ts_{\theta}(x)d x=E_{p(x)}[2tr(\triangledown_x s_{\theta}(x))]$，推导过程如下

$$
-2\int p(x)(\triangledown_x \log p(x))^Ts_{\theta}(x)d x=-2\int p(x)\sum_{i=1}^N\frac{\partial \log p(x)}{\partial x_i}s_{\theta_i}(x)d x=-2\int\sum_{i=1}^N\frac{\partial p(x)}{\partial x_i}s_{\theta_i}(x)d x
$$

$$
=2\sum_{i=1}^N -\int\frac{\partial p(x)s_{\theta_i}(x)}{\partial x_i}d x+\int p(x)\frac{\partial s_{\theta_i}(x)}{\partial x_i}d x=0+2\sum_{i=1}^N\int p(x)\frac{\partial s_{\theta_i}(x)}{\partial x_i}d x=2\int p(x)tr(\triangledown_x s_{\theta}(x))dx
$$

因此损失函数转化为
$$
L=E_{p(x)}[||s_{\theta}(x)||^2+2tr(\triangledown_x s_{\theta}(x))]
$$
仅与$s_{\theta}(x)$有关



具体训练流程如下，构造一个神经网络$G$，用$x$生成$s_{\theta}(x)$，每个样本的损失函数即$||s_{\theta}(x)||^2+2tr(\triangledown_x s_{\theta}(x))$



### 低概率密度区域陷阱优化——Noise Conditional Score Networks(NVSN)

**问题：**由于训练流程依赖于**$x$的样本分布**，换言之，$x$的分布总是集中在某些区域中，而大部分**低概率密度区域**压根就不会有样本产生，因此模型在低概率密度区域不会得到训练，也无法产生正确的$s_{\theta}(x)$，这会对**郎之万采样**过程产生很严重的影响。



**解决方法：**添加噪声扰动

通过给数据添加噪声扰动，增加高概率密度区域的面积，但这也会产生新的问题



**添加噪声扰动带来的问题：**

1.  **噪声扰动太小：**仍存在大量的低概率密度区域
2.  **噪声扰动太大：**破坏数据的原始分布



**解决方法：**添加不同程度的噪声
$$
x+\sigma_i z\sim p_{\sigma_i}(x)=\int p(y)N(x|y,\sigma_i^2\bold{I})dy
$$
新的分布可以理解为原分布$p(x)$对于$N(x'|x,\sigma_i^2\bold{I})$的加权

优化函数也变为优化**分数函数**使之尽可能的逼近每一个加噪扰动后的分数
$$
L=\sum_{i=1}^N\sigma_i^2 E_{p_{\sigma_i}(x)}[\vert\vert{\triangledown_x \log p_{\sigma_i}(x)-s_{\theta}(x,\sigma_i)||^2}]
$$

### NVSM的采样流程

在加入高扰动的区域进行采样，逐步减小扰动采样范围，最终收缩到$p(x)$附近区域

<iframe src="https://vdn6.vzuu.com/SD/032e7c74-5b7a-11ed-b178-4a4e462ccc3d.mp4?pkey=AAXGElgaEaoDNDjuRXDd4tn3AWLJ8RpyIppCyCi-JvQjkvZQHbzw5MHhRMPXM5hcoNegFFlz108j-Qs27G3CPwAt&c=avc.1.1&f=mp4&pu=078babd7&bu=078babd7&expiration=1683440034&v=ks6" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" height='285'> </iframe>



### **DDPM的分数表示形式**

根据***Tweedie Estimator***，对于一个高斯变量$z\sim N(z;\mu,\Sigma_Z)$，有：
$$
\mu_z=z+\Sigma_Z\triangledown \log p(z)
$$

对于**DDPM**的前向过程
$$
q_{\sigma}(x_t|x_0)\sim \mathcal{N}(x_t;\sqrt{\overline{\alpha}_t}x_0,(1-\overline{\alpha}_t)I)
$$
根据***Tweedie Estimator***，有
$$
\sqrt{\overline{\alpha}_t}x_0=x_t+(1-\overline{\alpha}_t)\triangledown \log p(x_t)
$$
而又因为

$$
x_t=\sqrt{\overline{\alpha}_t}x_{0}+\sqrt{1-\overline{\alpha}_t}z_t
$$
因此

$$
-\sqrt{1-\overline{\alpha}_t}z_t=(1-\overline{\alpha}_t)\triangledown \log p(x_t)
$$

$$
\triangledown \log p(x_t)=-\frac{z_t}{\sqrt{1-\overline{\alpha}_t}}
$$

可以看到，在DDPM中，分数函数其实就是所添加的噪声


$$
\hat{\mu}_{t-1}=\frac{1}{\sqrt{\alpha_t}}(x_t-\frac{\beta_t}{\sqrt{1-\overline{\alpha}_t}}\overline{z}_t)
$$

$$
\hat{\mu}_{t-1}=\frac{1}{\sqrt{\alpha_t}}(x_t-\beta_t \triangledown \log p(x_t))=\frac{1}{\sqrt{\alpha_t}}(x_t-s_{\theta}(x_t,t))
$$

相对应的模型优化函数也变为优化$s_{\theta}(x_t,t)$
$$
\arg \min_{\theta}(1-\alpha_i)[||s_{\theta}(x_t,t)-\triangledown \log p(x_t)||_2^2]
$$



## Score Based SDE

### 离散过程到连续过程

根据**郎之万采样：**
$$
x_{t+1}\leftarrow x_t+\delta \triangledown \log p(x_t) + \sqrt{2\delta}\varepsilon_t
$$
记$\Delta t = \delta$
$$
x_{t+\Delta t}-x_t=\triangledown \log p(x_t)\Delta t+\sqrt{2\Delta t}\varepsilon_t
$$
记$\log p(x_t)$为$f(x,t)$，$\sqrt{2}$为$g(t)$

则
$$
x_{t+\Delta t}-x_t= f(x,t)\Delta t+g(t)\sqrt{\Delta t}\varepsilon_t
$$
定义$w$是一个**布朗运动**，则
$$
w_{t+\Delta t}=w_t+N(0,\Delta t \bold{I})
$$

$$
\sqrt{\Delta t}\varepsilon_t=w_{t+\Delta t}-w_t
$$

（diffusion的加噪过程就是一个布朗运动）



令$\Delta t \rightarrow 0$

有
$$
dx=f(x,t)dt+g(t)dw
$$
该方程为**随机微分方程（SDE）**，可以看出，郎之万采样是它的一个特例，同时郎之万采样的**正过程和逆过程完全一致**（个人理解）



### 随机微分方程

$$
dx=f(x,t)dt+g(t)dw
$$

$f(.)$：漂移系数，$g(.)$：扩散系数，$w$：标准的布朗运动
$$
x_{t+\Delta t}-x_t= f(x,t)\Delta t+g(t)\sqrt{\Delta t}\varepsilon_t
$$
则
$$
x_{t}-x_{t-\Delta t}= f(x,t-\Delta t)\Delta t+g(t-\Delta t)(w_{t}-w_{t-\Delta t})
$$

$$
x_{t-\Delta t}-x_{t}= -f(x,t-\Delta t)\Delta t-g(t-\Delta t)(w_{t}-w_{t-\Delta t})
$$

$$
\frac{x_{t-\Delta t}-x_{t}}{-\Delta t}= f(x,t-\Delta t)+\frac{g(t-\Delta t)(w_{t}-w_{t-\Delta t})}{-\Delta t}
$$

由于是逆向过程，我们其实并不知道$w_{t-\Delta t}$的分布，但我们知道以下的先验
$$
w_{t-\Delta t}=w_{t}-N(0,\Delta t \bold{I})
$$

$$
p(w_{t-\Delta t})=p(w_{t}|\mu,-\Delta t \bold{I})
$$

根据**Tweedie Estimator**：
$$
\mu=w_t-\Delta t\triangledown \log p_t(w)
$$

$$
\triangledown \log p_t(w)=\frac{1}{p_t(w)}\frac{dp_t(w)}{dw}
$$

**（没推出来）**

因此：
$$
\frac{dx}{dt}=f(x,t)-
$$
**SDE**逆过程
$$
dx=[f(x,t)-g(t)^2\triangledown \log p_t(x)]dt+g(t)dw
$$

### VE SDE

SMLD转化为连续形式
$$
x_i=x+\sigma_i z
$$

$$
x_{i+1}=x+\sigma_{i-1}z
$$

二者相减：(不懂)
$$
x_i-x_{i-1}=\sigma_i z-\sigma_{i-1}z
$$

$$
x_i=x_{i-1}+\sqrt{\sigma_i^2-\sigma_{i-1}^2}z
$$



### VP SDE

将**DDPM**转变为连续形式：
$$
dx=-\frac{\beta(t)x}{2}dx+\sqrt{\beta(t)}dw
$$

叫做**VP SDE**























