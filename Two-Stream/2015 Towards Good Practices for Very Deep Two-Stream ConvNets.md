## Towards Good Practices for Very Deep Two-Stream ConvNets

论文公布源码，PyTorch版本

### Abstract

(1) 解释Deep convolutional networks为什么在视频领域取得的成果没有在图像领域的大

- 网络深度不够

- 可以用于训练的数据有限

（2）解决办法

- 设计了更深的网络

- spatial和temporal net进行预训练、使用小的学习率、更多数据增强方法、high drop out ratio

- 在ucf101上准确率可以达到91.4%


### 1 Introduction


### 2 Very Deep Two-stream ConvNets

- 提出深度双流神经网络

- 提出训练细节，可以很好地减少过拟合

- 描述了测试策略

#### 2.1 Network architectures

(1)GoogleNet(介绍GoogleNet结构，Inception module)

- GoogLeNet对网络中的传统卷积层进行了修改，提出了被称为Inception的结构，用于增加网络深度和宽度，提高深度神经网络性能。

- Inception V1——构建了1x1、3x3、5x5的 conv 和3x3的 pooling 的分支网络，同时使用 MLPConv 和全局平均池化，扩宽卷积层网络宽度，增加了网络对尺度的适应性；

- Inception V2——提出了 Batch Normalization，代替 Dropout 和 LRN，其正则化的效果让大型卷积网络的训练速度加快很多倍，同时收敛后的分类准确率也可以得到大幅提高，同时学习 VGG 使用两个3´3的卷积核代替5´5的卷积核，在降低参数量同时提高网络学习能力；

- Inception V3——引入了 Factorization，将一个较大的二维卷积拆成两个较小的一维卷积，比如将3´3卷积拆成1´3卷积和3´1卷积，一方面节约了大量参数，加速运算并减轻了过拟合，同时增加了一层非线性扩展模型表达能力，除了在 Inception Module 中使用分支，还在分支中使用了分支（Network In Network In Network）；

- Inception V4——研究了 Inception Module 结合 Residual Connection，结合 ResNet 可以极大地加速训练，同时极大提升性能，在构建 Inception-ResNet 网络同时，还设计了一个更深更优化的 Inception v4 模型，能达到相媲美的性能。

(2)VGGNet（介绍VGGNet）



(3)Very Deep Two-stream ConvNets

- spatial net使用一帧224 * 224 * 3的图片，类似于图像领域的目标识别

- temporal net使用10帧光流图，网络输入是224 * 224 * 20


#### 2.2 Network training

- UCF101数据集介绍：一共13320个视频片段，提供了三种数据划分方式（split），每种split包括10000个视频训练和3300个测试视频。

- 训练数据比较少，根据经验，作者发现了几个有助于训练深度网络的技巧。

(1)Pre-training for Two-stream ConvNets

- spatial net 利用ImageNet模型参数进行初始化

- temporal net 利用ImageNet模型初始化：第一，把提取到的光流信息利用线性函数映射到[0,255];第二，考虑到输入维度不同，20vs.3，根据channal平均第一个filter的数值，然后把平均的结果复制20次来初始化temporal net。

(2)Smaller Learning Rate

- temporal net：starts with 0.005，decreases to its 1/10 every 10,000 iterations, stops at 30,000 iterations

- spatial net：starts with 0.001,decreases to its 1/10 every 4,000 iterations, stops at 10,000 iterations

- 迭代次数不多是因为作为pre-trained the networks with the ImageNet models

(3)More Data Augmentation Techniques

- 除了cropping和flipping，作者又提出两种data augmentation的方法。

- 

(4)High Dropout Ratio

(5)Multi-GPU training


#### 2.3 Network testing

### 3 Experiments

### 4 Conclusions


** 空间流224 * 224 * 3  ；时间流 224 * 224 * 10 **

- 预训练：空间流在ImageNet上预训练，时间流中的光流转换为0-255灰度图，在ImageNet上预训练。

- 更小的learning_rate：时间为0.005，每1万次迭代减少1/10，3万次停止。空间为0.001，每4000次迭代减少1/10，1万次停止。

- more data—data argumentation：由于数据集过小的原因，采用裁剪增加数据集，

4个角和1个中心，还有各种尺度的裁剪。从{26,224,192,168}中选择尺度与纵横比进行裁剪。

- high dropout rate

- 多GPU训练
