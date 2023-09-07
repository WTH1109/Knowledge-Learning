# SVM

$SVM$目标：找到一个分割面，使得空间中每个点到分割面的距离大于$b$，$b$越大越好

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230426164934299.png" alt="image-20230426164934299" style="zoom: 67%;" />


$$
\frac{z_i(a^Ty_i+c)}{||a||}\geq b
$$


设$||a||b=1$

则问题转化为
$$
min\frac{||a||^2}{2},s.t.  \quad1-z_i(a^Ty_i+c)\leq0
$$
$a^Tx+c$即为构造的超平面

构造拉格朗日函数：
$$
L(a,\lambda)=\frac{||a||^2}{2}+\sum_{i=1}^n\lambda_i(1-z_i(a^Ty_i+c))
$$
对于凸优化问题



<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230426170359811.png" alt="image-20230426170359811" style="zoom: 60%;" />

可以写出拉格朗日函数

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230426170430231.png" alt="image-20230426170430231" style="zoom: 60%;" />

目标变为如下的优化问题
$$
\min_{x}\max_{\lambda,v}L(x,
\lambda,v) \quad(\lambda>0)
$$
这是因为当$x$落在可行域之外时，$f_i(x)>0$，$\lambda_i\rightarrow \infty$，当求关于$x$的最小值时会自动将该值忽略

当落在可行域之内时，$\lambda_i=0$，落在可行域之上时，$f_i(x)=0$



回到原式，等价于优化
$$
\min_{a}\max_{\lambda}L(a,\lambda)=\frac{||a||^2}{2}+\sum_{i=1}^n\lambda_i(1-z_i(a^Ty_i+c))
$$
关于$a$求偏导：
$$
\frac{\partial L(a,\lambda)}{\partial a}=a-\sum_{i=1}^n\lambda_iz_iy_i=0
$$
关于$c$求偏导：
$$
\sum_{i=1}^{n}\lambda_iz_i=0
$$
代入原式：
$$
L(a,\lambda)=\frac{(\sum_{i=1}^n\lambda_iz_iy_i)^T\sum_{i=1}^n\lambda_iz_iy_i}{2}+\sum_{i=1}^n\lambda_i(1-z_i(\sum_{i=1}^n\lambda_iz_iy_i)^Ty_i)
$$

$$
=\frac{(\sum_{i,j=1}^n\lambda_i\lambda_jz_iz_jy_i^Ty_j)}{2}+\sum_{i=1}^n\lambda_i-\sum_{i=1}^n\lambda_iz_i(\sum_{i=1}^n\lambda_iz_iy_i)^Ty_i)
$$

$$
=\sum_{i=1}^n\lambda_i-\frac{1}{2}\sum_{i,j=1}^n\lambda_i\lambda_jz_iz_jy_i^Ty_j
$$

