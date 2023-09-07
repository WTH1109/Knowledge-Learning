# 图像分割

## Unet

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403172047138.png" alt="image-20230403172047138" style="zoom: 50%;" />

属于是一种编码解码的架构

## Unet++

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403172505581.png" alt="image-20230403172505581" style="zoom:50%;" />

类似与desnet的思想

## Deeplab-V3

**SPP Layer（Spatial Pyramid Pooling空间金字塔池化）**

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403154706881.png" alt="image-20230403154706881" style="zoom: 50%;" />

**模型结构：**

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403161132539.png" alt="image-20230403161132539" style="zoom: 50%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403152626402.png" alt="image-20230403152626402" style="zoom: 50%;" />

空洞卷积优势：

- 更大的感受野
- 通过设置dilation rate参数，没有额外计算
- 可以按照参数扩大任意倍数的感受野
- 应用简单



应用实例：

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230403171820019.png" alt="image-20230403171820019" style="zoom:50%;" />