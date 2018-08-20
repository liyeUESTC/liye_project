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


    
