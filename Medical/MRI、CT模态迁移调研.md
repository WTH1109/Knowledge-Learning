



### MRI模态迁移

非增强生成增强

https://aapm.onlinelibrary.wiley.com/doi/10.1002/acm2.14120

Contrast-enhanced MRI synthesis using dense-dilated residual convolutions based 3D network toward elimination of gadolinium in neuro-oncology

影响因子：2.1

方法：残差网络＋U-net

<img src=".\image\MRI、CT模态迁移调研\image-20230918110820189.png" alt="image-20230918110820189" style="zoom:33%;" />

数据库：BraTS2021

- 模态：T1,T2,FLAIR,T1-CE
- 数据量：training set（n=800），validation set（n=200），test set（n=251）
- 尺寸： $180\times 180 \times 128$
- 链接：


复现效果：

![image-20231002164614406](./image/MRI%E3%80%81CT%E6%A8%A1%E6%80%81%E8%BF%81%E7%A7%BB%E8%B0%83%E7%A0%94/image-20231002164614406.png)

![image-20231002164712768](./image/MRI%E3%80%81CT%E6%A8%A1%E6%80%81%E8%BF%81%E7%A7%BB%E8%B0%83%E7%A0%94/image-20231002164712768.png)





利用对抗生成网络进行多模态CT的超分

https://ieeexplore.ieee.org/abstract/document/9098322

Transfer-Gan: Multimodal Ct Image Super-Resolution Via Transfer Generative Adversarial Networks

影响因子：6.6（ISBI）

判别器：VGG  生成器：ESRGAN

<img src="./image/MRI%E3%80%81CT%E6%A8%A1%E6%80%81%E8%BF%81%E7%A7%BB%E8%B0%83%E7%A0%94/image-20231002165807138.png" alt="image-20231002165807138" style="zoom: 25%;" />

数据库：未提供

- 模态：NCCT、CTP、CTA
- 数据量：9名患者，共4382张slice，分辨率$512\times 512$

代码：未提供





CycleGAN MRI和CT之间迁移

https://link.springer.com/chapter/10.1007/978-981-99-1414-2_34

CycleGAN Implementation on Cross-Modality Transfer Between Magnetic Resonance Image (MRI) and Computed Tomography (CT) Images

数据库：mri2ct

- 模态：CT、MRI
- 数据量：training set (n=367)     test set(n=67)
- 链接：[mri2ct (kaggle.com)](https://www.kaggle.com/datasets/delladominic/mri2ct/)

图例：

<img src="./image/MRI、CT模态迁移调研/image-20231003214340182.png" alt="image-20231003214340182" style="zoom:33%;" />

<img src="./image/MRI、CT模态迁移调研\image-20231003214455903.png" alt="image-20231003214455903" style="zoom: 33%;" />

方法：CycleGAN

<img src="./image/MRI%E3%80%81CT%E6%A8%A1%E6%80%81%E8%BF%81%E7%A7%BB%E8%B0%83%E7%A0%94/image-20231003175938289.png" alt="image-20231003175938289" style="zoom:15%;" />







https://www.sciencedirect.com/science/article/abs/pii/S0895611120300793

利用对抗学习的方式，用MRI迁移到CT域，之后进行CT和MRI的图像分割

MMTLNet: Multi-Modality Transfer Learning Network with adversarial training for 3D whole heart segmentation





用Cycle-GAN的方法通过MRI生成合成CT图像，其实就是在做分割

训练时MRI与CT不配对

https://link.springer.com/chapter/10.1007/978-3-030-33391-1_8

Cross-Modality Knowledge Transfer for Prostate Segmentation from CT Scans





# CT

Automated nonlinear registration of coronary PET to CT angiography using pseudo-CT generated from PET with generative adversarial networks