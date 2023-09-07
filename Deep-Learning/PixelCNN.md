# PixelCNN

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230425112630621.png" alt="image-20230425112630621" style="zoom: 50%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230425112714043.png" alt="image-20230425112714043" style="zoom: 50%;" />

## PixcelCNN++

标准的PixcelCNN每一个像素位置预测一个[0-255]的值，占用非常多的运算资源

同时具有如下的局限性：

对于预测的像素值128而言，实际上并不知道其究竟是接近127还是129

如果训练的图片中不包含摸个像素值，那么预测出的结果很有可能就是0



改进：

假设潜变量色度$v$存在连续的分布，通过$v$采样得到输出像素$x$

注：logistic分布的分布函数及为sigmod函数

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230425153327740.png" alt="image-20230425153327740" style="zoom: 67%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230425153231863.png" alt="image-20230425153231863" style="zoom: 67%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230425153309439.png" alt="image-20230425153309439" style="zoom:50%;" />



假设$v$服从混合logistic分布，即$v$是通过多个logistic分布叠加而成
$$
v\sim\sum_{i=1}^{K}\pi_i logistic(\mu_i,s_i)
$$
设$v$的分布函数为$F_{v}$，则
$$
F_{v}(x)=\sum_{i=1}^{K}\pi_i logistic(\mu_i,s_i)
$$

$$
p(x)=dF_v(x)
$$

对其离散化
$$
p(x|\pi,\mu,s)=F_v(x+0.5)-F_v(x-0.5)
$$
即
$$
p(x|\pi,\mu,s)=\sum_{i=1}^{K}\pi_i [\sigma((x+0.5-\mu_i)/s_i)-\sigma((x-0.5-\mu_i)/s_i)]
$$
可以看出$x$与$v$的某些值是可以一一映射的

$x=0$把$x-0.5$替换为负无穷，$x=255$把$x+0.5$替换为正无穷

