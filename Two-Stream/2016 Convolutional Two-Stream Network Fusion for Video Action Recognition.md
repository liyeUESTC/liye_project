## Convolutional Two-Stream Network Fusion for Video Action Recognition

** 源码公开，MATLAB版本 **

### Abstract

- fusion不一定在softmax层进行，可以在卷积层进行。不仅性能没有损失，而且节省了大量的参数

- 在最后一层进行融合要比在之前的层进行融合效果好，同时在分类预测层进行融合可以提升准确率

- 对时空领域中抽象卷积层特征进行pooling能够提升性能

### 1 Introduction


### 2 Related work


### 3 Approach

- 我们是根据文献[22]中的Two-stream架构设计的，该架构有两个缺点：

- （1）不能学习到像素在时间和空间的对应关系，因为融合只发生在分类预测层

- （2）时间尺度受到了限制，因为空间卷积处理单帧图片，时间卷积处理连续L张时间轨迹光流。

#### 3.1 Spatial fusion（spatial融合的几种方式介绍）

本节我们将研究不同的结构用于融合两个网络。

我们现在的目的是在特定的卷积层融合两个网络，并且通道对应的像素放到相同的位置。举例说明，想要分别刷牙和梳头发这两个动作，如果手周期性地在空间位置移动，然后时间网络可以识别这个动作，同时空间网络可以识别牙齿或者头发的位置，结合两个信息就可以分辨动作。

当两个网络有相同的分别率时，我们可以很简单的实现空间对应层的融合，只是简单的把一个网络堆叠到另一个网络上。然而，仍然存在的问题是一个网咯中的channel如何跟一个网络中的channel对应。

暂时假定空间网络中不同channel对应不同的脸部区域（嘴巴，头发），时间网络中一个channel对应每个脸部部位的周期运动。然后通过channel堆叠，下游的层的filter学习到通道之间的对应关系，以便更好地分辨动作。




- Sum fusion:对应项直接求和



- Max fusion：取同一channel里的最大值

- Concatenation fusion:串联层把两个网络的channel堆叠起来，保证信息完整

- Conv fusion：


![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180531105844.png)



#### 3.2 Where to fuse the networks

- 就像上面所看到的，融合可以发生在两个网络的任意位置，只有一个限制就是融合的两个输入必须有相同的维度，这个可以通过“upconvolutional”实现。
或者如果维度只是相似但不同，可以在比较小的map中用0做padding，以达到相同的维度。

- 以VGG为例，比较不同层的融合对参数数量的影响。在不同的卷积层进行融合对参数数量的影响大致相同，大多数的参数是在全连接层。两个网络的融合可以发生在两个层，如图2中的右侧。这也实现了最初的目标，每个网络通道之间的像素匹配，但是没有降低参数数量。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180531111803.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180530230831.png)

- 左边的例子展示了在conv4之后的fusion。而右边的例子展示了两次融合。分别在conv5和fc8。

- 右边的例子最后在第一次融合后，仍然独立保持了两个网络，一个是时空混合的网络，一个是独立的空间的网络。

#### 3.3 Temporal fusion

- 针对时间t的特征进行融合。针对时间输入的一种处理方法是平均网络的预测。

** 研究3D卷积或者3D Pooling时，可以忽略channel，channel和filter的维度相对应，一般不做特殊考虑，这样方便理解3D的卷积或者Pooling ** 

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180531163749.png)


- 3D Pooling： 用大小为 W' * H' * T'的cube进行3D的max-pooling，如上图b所示。需要注意的是这里没有跨channel的pooling，跨的是时间维度。

- 3D Conv + Pooling： 首先用D'个filter对四维的input x 进行卷积,D'个filter加起来的维度是W''* H'' * T' * D * D'，每个filter的维度是W''* H'' * T' * D，再加上一个biases，b的维度是D维，输出y = x_t * f+b，然后再接3D Pooling，filter的作用是对局部时空features进行融合。

- Disscussion：



#### 3.4 Proposed architecture

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180531112240.png)



#### 3.5 Implementation details


### 4 Evaluation

#### 4.1 Datasets and experimental protocols


#### 4.2 How to fuse the two streams spatially?


#### 4.3 Where to fuse the two streams spatially?


#### 4.4 Going from deep to very deep models


#### 4.5 How to fuse the two streams temporally?


#### 4.6 Comparison with the state-of-the-art


### 5 Conclusion

### 备注

（1）fusion介绍

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180530225637.png)

- fusion是一种提高模型性能的很好的方法，才参节kaggle比赛或者平时做项目上都是一个很常用的方法，kaggele比赛中，每一位参赛者的结果都是进行了fusion的。

- fusion可以在训练过程中，也可以在训练结束后。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180530230831.png)





