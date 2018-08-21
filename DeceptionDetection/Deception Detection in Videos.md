## Deception Detection in Videos 

## Abstract
通过学习例如vision、autio、text等不同形态特征的重要性，研究针对法庭实时审判视频的自动化谎言检测系统。
(1)vision，利用在low level video特征训练的分类器预测人类micro-expression。预测得到的micro-expression可以作为谎言预测的特征。
(2)IDT(Improved Dense Trajectory)特征，该特征广泛用于动作识别，同时该特征也有利于视频中的谎言预测。
(3)audio领域的MFCC特征也可以有效提升性能，然而打字本信息对我们的系统没有很大帮助。
(4)比起state-of-the-art方法中使用人工标注的micro-expression做谎言预测，我们完全自动化的方法提升了5%的准确率。
(5)当我们结合人工标注的micro-expression，我们的AUC可以达到0.922，相比与之前10次交叉验证所得到的0.877。


## Introduction



## Related Work


## Method

mutil-modal feature extraction -> feature encoding -> classification

### Multi-Modal Feature Extraction

#### Motion Features:
我们的输入源是视频，视频中一个人在做真实或者虚假的陈述。视频中的场景是不受限制的，所以目标脸部并不是一直在正面或者中心的。因此我们利用IDT特征，考虑到该特征在动作识别，特别是无限制条件下的突出表现。
考虑到我们想要检测micro-expression，描述符应该表示变化的动作信息而不是静止动作，该变化的动作信息是由MBH捕获的。    


### Audio Features

我们使用MFCC特征作为audio特征。
首先，估计每个短帧的功率谱周期图，然后变形到Mel frequency scale,最后计算出log-Mel光谱的DCT（离散余弦变换）。然后针对每一段视频，我们有一系列对应于短间隔的MFCC特征。
提起到MFCC特征后，我们利用GMM（Gaussian Mixture Model）为所有的训练视频建立一个声音特征字典。我们平等看待所有的audia特征，然后利用我们的特征编码方法编码整个序列。
这是因为我们对演讲内容不感兴趣，而是对隐藏在audio领域的欺骗线索感兴趣。

### Transcript Features
针对每段视频，我们利用Glove方法编码视频中的整个单词集，变为一个固定长度的向量。Glove是一种利用向量表示单词的无监督的学习算法。
（未完）

### Feature Encoding

考虑到每段视频中的特征数量是不同的，我们利用了一个Fisher Vector聚集一个数量可变的特征到固定长度的向量。Fisher Vector结合了生成模型和判别模型的优势，被广泛用于其他的计算机视觉实验，例如图像分类，动作识别。
（Fisher Vertor具体公式实现）


### Facial Micro-Expression Prediction

前面介绍的多模态特征都是low-level features，这里我们介绍用于代表micro-expression的high-level features.根据(Ekman et
al. 1969; Ekman 2009)，facial micro-expression在预测虚假行为中扮演重要角色。为了研究他们的有效性，(Perez-Rosas et al. 2015)手动标注facial expression，然后使用来源于ground truth annotations的二值特征预测谎言。他们证明最重要的5种微表情是：Frowning, Eyebrows raising,Lip corners up,Lips protruded, Head Side Turn(皱眉，眉毛抬起，唇角向上，嘴唇突出，头侧转弯)。
我们使用low-level视觉特征训练micro-expression检测器，然后使用micro-expression的预测得分作为high-level特征来做虚假预测。我们把数据库中的每段视频分为固定持续时间的视频片段，然后利用micro-expression标签注释这些clips.每个clip持续4s.

### Deception Detection

Fisher Vector编码low level features，视频级别的得分向量被用于训练4个二值虚假分类器。其中的三个分类器是基于visual、auditory、text架构的GMM，第四个用降采样得分向量做micro-expression检测器。
（得分融合计算公式）

## Experiment and Result

### Dataset
我们在真实虚假检测数据集上Perez-Rosas et al.2015)评估我们的欺骗自动检测方法。该数据集包含了121法庭审判视频片段。在审判数据库中的视频是不受限制的。因此，我们需要处理在人物角度、视频质量变化以及背景噪声诸多方面的不同。如下图所示：
（插入图片）

我们使用了104个视频子集，从121个视频数据库中挑选出来，包括50个真实视频以及54个欺骗视频。剪辑后的视频要么具有重要的场景改变，要么有人物编辑。在下述试验中显示，我们不会展示在(Perez-Rosas et al. 2015)中的结论。我们重新使用了（referred to Ground Truth micro-expressions)方法，在我们的训练集合测试集上，为了避免过拟合识别，而不是欺骗线索。
数据集中仅仅包含58个身份，少于视频的数量，所以同一身份表现了欺骗和真实。这意味着，如果同一个人的视频被包含在训练集和测试集，欺骗检测方法则退化为对人的重新定义。为了避免这个为题，我们利用身份进行了10次交叉验证，而不是视频，对于所有的试验。在测试集中的身份将不会出现在训练集中。












    
