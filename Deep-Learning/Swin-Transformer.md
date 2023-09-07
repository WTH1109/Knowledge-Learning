# Swin-Transformer(Shift-Windows)

使用滑动窗口的阶梯视觉Transformer

vit-transformer面临的挑战：视觉实体的变化尺度大，像素分辨率高

模型结构：

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221110163531489.png" alt="image-20221110163531489" style="zoom: 50%;" />

相较于vit-transformer，每经过一个stage，会先由Patch Merging模块将2×2的patch合并起来，同时将C变为2C，类似于VGG降维的思想

平移窗口：

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221110173150910.png" alt="image-20221110173150910" style="zoom: 50%;" />

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20221110175049387.png" alt="image-20221110175049387" style="zoom: 67%;" />