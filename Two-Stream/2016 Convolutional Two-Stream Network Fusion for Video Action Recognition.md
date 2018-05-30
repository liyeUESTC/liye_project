## Convolutional Two-Stream Network Fusion for Video Action Recognition

** 源码公开，MATLAB版本 **

### Abstract

- fusion不一定在softmax层进行，可以在卷积层进行。不仅性能没有损失，而且节省了大量的参数

- 在最后一层进行融合要比在之前的层进行融合效果好，同时在分类预测层进行融合可以提升准确率

- 时空领域中抽象卷积层特征的pooling能够提升性能

### 1 Introduction


### 2 Related work


### 3 Approach

- fusion介绍

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180530225637.png)

- fusion是一种提高模型性能的很好的方法，才参节kaggle比赛或者平时做项目上都是一个很常用的方法，kaggele比赛中，每一位参赛者的结果都是进行了fusion的。

- fusion可以在训练过程中，也可以在训练结束后。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180530230831.png)

#### 3.1 Spatial fusion（spatial融合的几种方式介绍）

- Sum fusion:对应项直接求和

- Max fusion：最大融合

- Concatenation fusion:公式

- Conv fusion：


#### 3.2 Where to fuse the networks



#### 3.3 Temporal fusion



#### 3.4 Proposed architecture


#### 3.5 Implementation details


### 4 Evaluation

#### 4.1 Datasets and experimental protocols


#### 4.2 How to fuse the two streams spatially?


#### 4.3 Where to fuse the two streams spatially?


#### 4.4 Going from deep to very deep models


#### 4.5 How to fuse the two streams temporally?


#### 4.6 Comparison with the state-of-the-art


### 5 Conclusion





