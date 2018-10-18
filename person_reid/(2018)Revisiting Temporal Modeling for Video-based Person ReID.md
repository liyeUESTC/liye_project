## Revisiting Temporal Modeling for Video-based Person ReID (2018)


### Abstract

一个典型的基于视频的行人重识别系统包含以下三个部分：
(1)image-level feature extractor,特征提取器，例如CNN，
(2)聚集时间特征的时间模型方法，temporal modeling
(3)loss函数

论文中比较了四种方法：

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/71.png)


temporal pooling
temporal attention
RNN
3D convnets

重新提出了新的方法，利用temporal convolution在视频帧中提取时间信息。

在MARS数据集上进行测试，发现准确率要高于现有的方法。

### 1 Introduction



本论文采用
研究不同的temporal modeling方法，通过固定
特征提取器（ResNet-50）和loss函数（triplet loss和softmax cross-entropy loss）,
比较了temporal pooling, temporal attention, RNN, 3D convnets四种方法。

总结，主要有两方面贡献：
（1）深入研究了四种temporal modeling方法。
（2）提出了新奇的temporal-conv方法。



### 2 Related work

- Video-based persion reID(RNN based & temporal attention based)
```
(1)McLanghlin et al. 第一次提出了通过RNN来提取frams间的时间信息，RNN cell输出的平均作为clip level表示。
(2)Yan et al. 利用RNN编码系列特征，final hidden state作为video的表示。
(3)Liu et al. 设计Quality Aware Network(QAN), 是一种attention weighted average,
   聚集时间特征.attention scores 是frame-level的feature map产生的。
(4)Zhou et al. & Xu et al. 提出利用temporal attention和RNN编码视频。
(5)Chung et al. 提出基于RGB和flow的双流网络，简单的用temporal pooling聚集特征。
(6)Zheng et al. 建立的新的MARS数据集，用于基于视频的行人重识别，成为了该test的标准benchmark.
```
- Image-based person reID(image spatial modeling & loss function for metric learning)
```
(1)Su et al. & Zhao et al. 利用人体关节解析图片，进而融合空间特征
(2)Zhao et al. 提出了用于处理身体部分错位问题的部分对齐表示方法。
(3)Hermans et al. 提出了修改的triple loss，能够达到目前最好的效果。
```

- Video temporal analysis
```
(1)Karpathy et al. 设计CNN网络用于提取frame-level feature, 同时利用temporal pooling方法聚集features
(2)Tran et al. 提出了3DCNN网络用于从视频中提取spatio-temporal features
(3)Hara et al. 尝试使用3D卷积的ResNet
(4)Gao et al. 提出了一种时空边界回归网络在long videos中定位动作。
```

### 3 Methods
```
整个系统可以分为三个部分:

(1)针对video clips的特征提取器

(2)用于优化特征提取器的loss函数

(3)用于匹配query video和gallery videos的方法

一段video首先被分为没有重复的clips片段，每个片段包含T帧。clip编码器以每个clip作为输入，以D维的特征向量fc作为输出。整个视频的特征是每一个clip特征的平均。
```
#### 3.1 Video Clip Encoder

针对video clip编码器，我们考虑两种类型的卷积网络
(1)3DCNN,输入包含n帧的clip c，输出特征向量fc
(2)2DCNN和temporal aggregation method，针对每一帧，提取fc',得到n个fc'，然后利用temporal modeling方法融合fc'，得到一个特征向量fc

3D CNN.
论文采用3D ResNet模型，即在ResNet框架中采用3D卷积核，用行人重识别的输出代替了分类层的输出，同时采用了在Kinetics上的预训练参数。
2D CNN.
论文采用了标准残差模型作为image level特征提取器。针对n帧clip，分别输入每一帧的image，得到n个特征向量，即n * D矩阵，n是clip的长度，D是image level feature维度。然后利用temporal aggregation方法进行特征聚集，得到clip level的特征fc.

特别地，论文测试了三种不同的temporal modeling方法：


(1)temporal pooling

max pooling <a href="https://www.codecogs.com/eqnedit.php?latex=f_{c}=max_{t}(f_{c}^{t})" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_{c}=max_{t}(f_{c}^{t})" title="f_{c}=max_{t}(f_{c}^{t})" /></a>

average pooling <a href="https://www.codecogs.com/eqnedit.php?latex=f_{c}=\frac{1}{T}\sum_{t=1}^{n}f_{c}^{t}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?f_{c}=\frac{1}{T}\sum_{t=1}^{n}f_{c}^{t}" title="f_{c}=\frac{1}{T}\sum_{t=1}^{n}f_{c}^{t}" /></a>

(2)temporal attention
通过网络计算得到n个feature map的重要系数。

"spatial conv + FC": 
input(T,w,h,2048) -> spatial conv(w,h,2048,dt) -> FC(input:dt,output1) -> output(T,1)

"spatial + temporal conv" (存在问题？)
input(T,w,h,2048) -> spatial conv(w,h,2048,dt) -> temporal conv(3,dt,1) -> s_c^t -> 

(3)RNN











#### 3.2 Loss Functions

triplet loss function + softmax cross-entropy loss function
(详细介绍)

batch hard triplet loss function


softmax cross-entropy loss function





#### 3.3 Similarity Calculation for Testing



### 4 Evaluation


#### 4.1 Evaluation Settings

- Metric.
标准评估矩阵

- Dataset.
MARS数据集

- Implementations.
```
在ImageNet预训练的标准Res-Net50用于2D CNN
在Kinectics预训练的3D Res-Net50用于3D CNN视频编码
video frames resize 到224* 112
用Adam优化网络
batch size = 32,如果显存溢出，则把batch size调整到最大
设置P=4,每个身份选取4个样本
针对不同的模型，学习率设置为0.0001和0.0003
```
- Image-based baseline models
```
论文提供image-based baseline model用于测试temporal modeling的效率
clip的长度设置为T=1，同时没有使用temporal modeling方法
```

#### 4.2 Experiments on MARS

分别使用3D CNN,temporal pooling, temporal attention, RNN, 来讨论实验结果。



#### 4.3 



### 5 Conclusion







