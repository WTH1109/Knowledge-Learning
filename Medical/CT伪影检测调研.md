# CT伪影检测调研

## Deep-learning-based CT motion artifact recognition in coronary arteries

链接：https://hannes.nickisch.org/papers/conferences/elss18motion_artifact.pdf

摘要：运动伪影的检测和随后的校正对于使用心脏CT的无创冠状动脉造影的高诊断价值至关重要。然而，运动校正算法具有大量的计算占用空间和可能的失败模式，这保证了运动伪影检测步骤以首先决定是否需要运动校正。我们研究了如何通过深度学习方法准确预测冠状动脉中的运动伪影。通过在滤波反投影 (FBP) 算法中创建和集成人工运动矢量场来模拟心脏运动的前向模型，使我们能够从9个前瞻性ECG触发的高质量临床病例中生成训练数据。我们训练卷积神经网络 (CNN)，对2D无运动和受运动干扰的冠状动脉横截面图像进行分类，并通过四重交叉验证达到94.4% + 2.9% 的分类精度

问题：用于处理运动伪影，并且数据集是自己造的，伪影图像是用算法生成的，模型直接用微软工具包

启发：将任务分解成patch

## Deep Learning Cross-Phase Style Transfer for Motion Artifact Correction in Coronary Computed Tomography Angiography

[IEEE Access](https://ieeexplore.ieee.org/xpl/RecentIssue.jsp?punumber=6287639) 

 30 April 2020

链接：https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9082676

摘要：在冠状动脉计算机断层扫描血管造影 (CCTA) 中可能由于心跳而出现运动伪影，并阻碍临床医生对冠状动脉疾病的诊断。因此，需要冠状动脉的运动伪影校正以更准确地量化疾病的风险。我们提出了一种基于深度学习的CCTA运动伪影校正方法。由于无运动的冠状动脉图像 (监督深度学习中所需的地面真实数据) 在医学上是无法实现的，因此我们将样式转移方法应用于从全相位4D计算机断层扫描 (CT) 裁剪的2D图像补丁以合成这些图像。然后，我们使用此合成地面实况 (SynGT) 训练卷积神经网络 (CNN) 进行运动伪影校正。在测试期间，经训练的网络的输出运动校正的2D图像块通过体积插值被重新插入到3dct体积中。使用体模和临床数据对所提出的方法进行了评估。phantom研究在定量性能方面证明了与其他方法可比的结果，并且在计算时间方面优于这些方法。对于临床数据，提出了基于度量测量的定量分析，以确认运动伪影的校正。此外，观察者研究发现，通过应用所提出的方法，运动伪影显着减少，并且冠状动脉的边界更加清晰，观察者之间具有很强的一致性 (x = 0.78)。最后，使用商业软件对所提出方法的原始和所得CT体积进行的评估显示，跟踪的冠状动脉长度显着增加



（a）模拟伪影 （b）临床伪影

<img src="./image/CT%E4%BC%AA%E5%BD%B1%E6%A3%80%E6%B5%8B%E8%B0%83%E7%A0%94/image-20231206213127410.png" alt="image-20231206213127410" style="zoom:33%;" />

4D图像，包含时间信息，用运动较慢时刻去指导较快时刻的图像，合成当前时刻的GT，然后用这个GT训练MAC-net

<img src="./image/CT%E4%BC%AA%E5%BD%B1%E6%A3%80%E6%B5%8B%E8%B0%83%E7%A0%94/image-20231206214405706.png" alt="image-20231206214405706" style="zoom: 50%;" />



## Motion Estimation and Correction in Cardiac CT Angiography Images using Convolutional Neural Networks

https://hannes.nickisch.org/papers/articles/lossau19motion.pdf

[Computerized Medical Imaging and Graphics](https://www.sciencedirect.com/journal/computerized-medical-imaging-and-graphics)

2019.09

摘要：心脏运动伪影经常降低冠状动脉计算机断层扫描血管造影 (CCTA) 图像的可解释性，并可能导致误解或排除冠状动脉疾病 (CAD) 的诊断。本文提出了一种新颖的运动补偿方法，该方法通过CT数据 (COMPACT) 中的斑块分析来处理冠状动脉运动估计。首先，监督学习所需的数据是由CT数据的冠状动脉运动前向伪影模型 (CoMoFACT) 生成的，该模型通过分步拍摄采集proto-col将模拟运动引入19个无伪影的临床CT病例。其次，训练卷积神经网络 (cnn) 以基于冠状动脉伪影外观从2.5D图像块估计潜在的2D运动向量。在使用计算机模拟血管的体模研究中，cnn预测运动方向和运动幅度，平均测试精度分别为13.37 ° £ 1.21 ° 和0.77 £ 0.09毫米。在具有模拟运动的临床数据上，实现了34.85 °/2.09 ° 和1.86 0.11毫米的平均测试精度，由此运动方向预测的精度随着运动幅度而增加。经训练的cnn被集成到包括距离加权的运动向量外推的迭代运动补偿流水线中。在具有真实心脏运动伪影的十二个临床病例中交替运动估计和补偿导致显著降低的伪影水平，尤其是在具有严重伪影的图像数据中。在四项观察者研究中，以五点李克特量表对没有MC的3.08 0.24和具有紧凑型MC的2.28 0.29的平均伪影水平进行了评分。

<img src="./image/CT%E4%BC%AA%E5%BD%B1%E6%A3%80%E6%B5%8B%E8%B0%83%E7%A0%94/image-20231206215350798.png" alt="image-20231206215350798" style="zoom:33%;" />

<img src="./image/CT%E4%BC%AA%E5%BD%B1%E6%A3%80%E6%B5%8B%E8%B0%83%E7%A0%94/image-20231206215414561.png" alt="image-20231206215414561" style="zoom:33%;" />

## Motion artefact reduction in coronary CT angiography images with a deep learning method

https://link.springer.com/content/pdf/10.1186/s12880-022-00914-2.pdf

<img src="./image/CT%E4%BC%AA%E5%BD%B1%E6%A3%80%E6%B5%8B%E8%B0%83%E7%A0%94/image-20231206215654463.png" alt="image-20231206215654463" style="zoom:33%;" />

**摘要**
**背景:** 这项研究的目的是研究像素到像素生成对抗网络 (GAN) 消除冠状动脉CT血管造影 (CCTA) 图像中运动伪影的能力。
方法: 回顾性研究了97例接受单心动周期多相CCTA的患者，并获得了原始CCTA图像和快照冻结 (SSF) CCTA图像。对右冠状动脉 (RCA) 进行了研究，因为其运动伪影是所有冠状动脉伪影中最突出的。将获得的数据分为40名患者的训练数据集、30名患者的验证数据集和27名患者的测试数据集。使用SSF CCTA图像作为目标，训练像素到像素GAN以从原始CCTA成像数据生成改进的CCTA图像。通过结构相似性 (SSIM)，骰子相似系数 (DSC) 和圆形度指数来评估GAN去除运动伪影的能力。此外，由两名放射科医生视觉评估图像质量。

**结果:** GAN生成的图像的圆形度明显高于RCA的原始图像 (0.82 £ 0.07 vs.0.74 £ 0.11，p< 0.001)，GAN生成的图像和SSF图像之间没有显着差异 (0.82 0.07 vs. 0.82 £ 0.06，p = 0.96)。此外，GAN生成的图像实现了0.87 £ 0.06的SSIM，明显优于0.83 £ 0.08的原始图像 (p <0.001)。DSC的结果表明，GAN生成的图像和SSF图像之间的重叠显著高于GAN生成的图像和原始图像之间的重叠 (0.84 £ 0.08对0.78 £ 0.11，p< 0.001)。pRCA和mRCA的GAN生成的CCTA图像的运动伪影得分明显高于原始CCTA图像 (3 [4-3] vs 4 [5-4]，p = 0.022；3 [3-2] 与5[5-4]，p< 0.001)。

**结论:** GAN可以显着减少RCA中段CCTA图像中的运动伪影，并有可能作为消除冠状动脉CCTA图像中运动伪影的新方法。
