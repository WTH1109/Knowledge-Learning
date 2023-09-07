# 人工智能在CT、MRI模态增强的应用

### 1. Synthesizing MR Image Contrast Enhancement Using 3D High-resolution ConvNets

[笔记]: ../paper/SynthesizingMRImageContrastEnhancementUsing3DHigh-resolutionConvNets.md

时间：2022

发表：[IEEE Transactions on Biomedical Engineering](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=10)，影响因子4.756

总结：T1+T2+ADC生成T1增强，预处理利用了FSL工具去除头骨，提出focal增强对于肿瘤区域的兴趣，PSNR在28dB

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230725155733706.png" alt="image-20230725155733706" style="zoom: 33%;" />

### 2.Magnetic resonance imaging contrast enhancement synthesis using cascade networks with local supervision

[笔记]: ../paper/Magnetic_resonance_imaging.md

时间：2022

发表：[ MEDICAL PHYSICS](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=10)，影响因子4.756

总结：利用T1生成CE-T1，并用T1的肿瘤区域轮廓图来进行监督，从而加强模型对于肿瘤区域的关注，Synthesizing MR Image Contrast ......这篇文章被选为了对比算法

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230725180745700.png" alt="image-20230725180745700" style="zoom: 33%;" />

### 3.Deep learning-based 3D MRI contrast-enhanced synthesis from a 2D noncontrast T2Flair sequence

发表：[ MEDICAL PHYSICS](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=10)，影响因子4.756

总结：T2Flair生成T2Flair增强，PSNR32.25dB（全部），24.93dB（肿瘤）

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230725195737718.png" alt="image-20230725195737718" style="zoom: 33%;" />

### 4.Contrast-enhanced MRI synthesis from non-contrast MRI using attention CycleGAN

发表：SPIE Medical Imaging 

下载不了文章



### 5.Attention-Based Multi-Scale Generative Adversarial Network for synthesizing contrast-enhanced MRI

发表：Engineering in Medicine & Biology Society（1.5260）

总结：连模态是什么都没说



配准：

<img src="https://wth-markdown-image.oss-cn-beijing.aliyuncs.com/markdown_img/image-20230725223845405.png" alt="image-20230725223845405" style="zoom:50%;" />

医学数据集：

[sfikas/medical-imaging-datasets: A list of Medical imaging datasets. (github.com)](https://github.com/sfikas/medical-imaging-datasets)
