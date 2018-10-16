## Revisiting Temporal Modeling for Video-based Person ReID (2018)


### Abstract

一个典型的基于视频的行人重识别系统包含以下三个部分：
(1)image-level feature extractor,特征提取器，例如CNN，
(2)聚集时间特征的时间模型方法，temporal modeling
(3)loss函数

论文中比较了四种方法：
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
(2)Zhao et al. 提出了用于处理身体部分错误问题的部分对齐表示方法。
(3)Hermans et al. 提出了修改的triple loss，能够达到目前最好的效果。
```


### 3 Methods





### 

