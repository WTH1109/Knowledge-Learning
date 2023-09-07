# Fisher线性判别$LDA$

存在如下的样本集
$$
D=\{(x_i,y_i)|x_i\in R,y_i\in\{c_1,c_2,...,c_n \} \}
$$
现在希望能够找到一个$w$，使得将$x_i$投影在向量$w$上后，可以将每一类清楚地分开

目标：找到一个$w$，使得类内间隔尽可能小，类间间隔尽可能大

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230410165832359.png" alt="image-20230410165832359" style="zoom:50%;" />

若$w$是单位向量，则$w^Tw=1$，将$x_i$投影至$w$上，得到$x_i'$
$$
x_i'=(\frac{w^Tx_i}{w^Tw})w=w^Tx_iw=a_iw
$$
因此$w^Tx_i$就是$x_i$映射后的坐标

分组 $y_i$  $\Leftarrow$  $a_i$



第$k$类的均值为：
$$
m_k=\frac{1}{|c_k|}\sum_{x_i\in D_k} a_i=\frac{1}{|c_k|}\sum_{x_i\in D_k} w^Tx_i=w^T\frac{1}{|c_k|}\sum_{x_i\in D_k} x_i=w^T\mu_k
$$
投影点的均值与均值的投影相同

我们希望最大化$m_i-m_j$



怎样定量描述同类之间的距离？

方差$\Rightarrow$散度

方差：$\frac{1}{n}\sum(x_i-m_i)^2$，代表平均偏差

散度：$S_i^2=n_i\sigma_i^2$，代表累计偏差



因此上述问题可以被描绘为一个最优化问题
$$
\begin{align*}
\begin{split}
 \left \{
\begin{array}{ll}
    max\sum_{i,j}|m_i-m_j|,                    \\
 	min\sum S_i^2\\
\end{array}
\right.
\end{split}
\end{align*}
$$
Fisher定义优化目标函数
$$
J(w)=\frac{\sum_{i,j}(m_i-m_j)^2}{\sum S_i^2}
$$
$w^*=\arg \max_w J(w)$



### 矩阵描述

$$
(m_i-m_j)^2=(w^T(\mu_i-\mu_j))^2=w^T(\mu_i-\mu_j)w^T(\mu_i-\mu_j)
$$

$$
=w^T(\mu_i-\mu_j)(\mu_i-\mu_j)^Tw=w^TS_{b_{ij}}w
$$

$$
S_b=\sum_{i,j}S_{b_{ij}}=\sum_{i,j}(\mu_i-\mu_j)(\mu_i-\mu_j)^T
$$

$S_b$为类间散度矩阵
$$
S_i^2=\sum_{x_j\in D_i}(a_j-m_i)^2=\sum_{x_j\in D_i}(w^Tx_j-w^T\mu_i)^2=\sum_{x_j\in D_i}(w^T(x_j-\mu_i))^2
$$

$$
=\sum_{x_j\in D_i}w^T(x_j-\mu_i)(x_j-\mu_i)^Tw=w^T\sum_{x_j\in D_i}(x_j-\mu_i)(x_j-\mu_i)^Tw=w^TS_{w_i}w
$$

$$
S_w=\sum_{i,x_j\in D_i}S_{w_i}=\sum_{i,x_j\in D_i}(x_j-\mu_i)(x_j-\mu_i)^T
$$

$S_w$为类内散度矩阵

因此
$$
w^*=\arg \max_w J(w)=\arg \max_w \frac{w^TS_bw}{w^TS_ww}
$$

$$
\ln J(w)=\ln{w^TS_bw}-\ln{w^TS_ww}
$$

$$
\ln J(w)'=\frac{2S_bw}{w^TS_bw}-\frac{2S_ww}{w^TS_ww}
$$

由此得到：
$$
S_bw(w^TS_ww)=S_ww(w^TS_bw)
$$

$$
S_bw=S_ww(\frac{w^TS_bw}{w^TS_ww})
$$

$$
S_bw=S_wwJ(w)=\lambda S_ww
$$

$$
S_w^{-1}S_bw=\lambda w
$$

因此要求$w$，相当于求$S_w^{-1}S_b$的特征向量





步骤：

1）求均值，类间散度，类内散度

2）求$S_w^{-1}S_b$的特征向量