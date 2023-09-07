# VAE-变分自动编码器

基本思想：

$x$是根据一个隐变量$z$所生成的，训练时通过一个后验网络$q_{\phi}(z|x)$通过$x$生成$z$

推理时根据$p_{\theta}(x|z)$通过$z$去预测$x$

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317163840186.png" alt="image-20230317163840186" style="zoom: 33%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230317164727587.png" alt="image-20230317164727587" style="zoom: 33%;" />

