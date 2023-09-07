# Transformer

## Attention is all you need

### 总体框图

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103104539728.png" alt="image-20221103104539728" style="zoom: 33%;" />

### 1.位置编码

输入经过embedding后需要进行位置编码

编码方式：
$$
PE_{(pos,2i)}=sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})
$$

$$
PE_{(pos,2i+1)}=cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}})
$$

原理：
$$
\begin{equation}
\begin{split}
PE_{(pos+k,2i)}=sin(\frac{pos+k}{10000^{\frac{2i}{d_{model}}}})=sin(\frac{pos}{10000^{\frac{2i}{d_{model}}}})cos(\frac{k}{10000^{\frac{2i}{d_{model}}}})+\\
cos(\frac{pos}{10000^{\frac{2i}{d_{model}}}})sin(\frac{k}{10000^{\frac{2i}{d_{model}}}})
\\
=PE_{(pos,2i)}PE_{(k,2i+1)}+PE_{(pos,2i+1)}PE_{(k,2i)}
\end{split}
\end{equation}
$$
这说明了通过这样位置编码所得到的结果，绝对位置中包含了相对位置的信息

思考：为什么要用相加的方式，而不是concat的方式？相加难道不会损失掉一些信息吗？

### 2.注意力机制

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103110016791.png" alt="image-20221103110016791" style="zoom: 33%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103112512872.png" alt="image-20221103112512872" style="zoom: 50%;" />

The input consists of queries and keys of dimension $d_k$, and values of dimension $d_v$. 

输入由查询向量$Q$，维度为$d_k$的键值向量$K$，维度为$d_v$的值向量构成

$d_k$由embed_dim // num_head得到
$$
Attention(Q,K,V)=softmax(\frac{QK^T}{\sqrt{d_k}})V
$$
公式理解：$QK^T$是在计算输入与句子中各值的相关系数，之后用该相关系数对于输出值进行加权

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103111358386.png" alt="image-20221103111358386" style="zoom:50%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103110216561.png" alt="image-20221103110216561" style="zoom:33%;" />

### 3.多头注意力机制

一组参数可以得到一组$Q,K,V$，多组参数可以得到多组$Q,K,V$，将多组输出concat起来得到多头输出

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103112752119.png" alt="image-20221103112752119" style="zoom:67%;" />

最后别忘了把输出做一个线性变换保证跟输入维度一致（后面还要用残差）

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103113340463.png" alt="image-20221103113340463" style="zoom:50%;" />

### 4.layer normal

为什么不用batch normal？

BN：对于一组batch将它们同一维度上的值进行归一化处理

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221103113910634.png" alt="image-20221103113910634" style="zoom:67%;" />

优点：缓解梯度饱和

缺点：batch较小时效果较差

在RNN中表现不佳：因为RNN的输入是动态的

### 5.前馈神经网络

Linear + ReLU + Linear