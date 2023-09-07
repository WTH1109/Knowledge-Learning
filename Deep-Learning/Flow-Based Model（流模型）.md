# Flow-Based Model（流模型）

## **Change of Variable Theorem**（变量变换定理）

**问题提出：**已知$z\sim \pi(z)$，$x=f(z)$，$f$是可逆函数，如何求$x$的概率密度函数$p(x)$？

​	微观上看，**局部**区域的**概率密度函数的面积**不随变量代换发生改变

​	因此，$p(x)dx=\pi(z)dz$，由于$dx=\frac{\partial f}{\partial z}dz$
$$
p(x)J_f=\pi (z)
$$

$$
p(x)=\pi(f^{-1}(z))J_f^{-1}
$$

## **Normalizing Flows**

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230626120129061.png" alt="image-20230626120129061" style="zoom: 50%;" />

**方法描述：**基于一连串可逆变换将简单分布转换为复杂分布

**具体做法：**

假设 $z_{i-1}\sim p_{i-1}(z_{i-1})$，$z_{i}=f_{i}(z_{i-1})$

根据**变量变化定理**，$p_i(z_i)=J_{f_i}^{-1}p_{i-1}(z_{i-1})=p_{i-1}(z_{i-1})|\det\frac{d f_i}{d z_{i-1}}|^{-1}$

由此可以得到：
$$
\log p(x)=\log z_0 - \sum_{i=1}^{K}\log |\det \frac{d f_i}{d z_{i-1}}|
$$
**优化方法：**

优化对数似然，$D$为训练数据集，$x$为$D$中的样本
$$
L(D)=-\frac{1}{|D|}\sum_{x\in D} \log p(x)
$$




