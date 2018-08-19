## Two-stream convolutional networks for action recognition in videos
论文没有公开源码

** 核心思想：**
```
首次提出Two-Stream网络，主要分两个流，均为二维卷积操作：空间流处理静止图像帧，得到空间特征；
时间流处理连续多帧光流，得到时间特征。两个流最后经过softmax，做分类分数的
融合，可以采用平均法或者SVM。
```

### 1 Introduction
                   
### 2 Two-stream architecture for video recognition

![ Two-stream architecture ](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180520233723.png)

（1）The spatial part: individual frame, carrier information about scenes and objects depicted in the video

（2）The temporal part: multi-frame, conveys the movement of the observer(the camera) and the objects

（3）late fusion(融合)： averaging or multi-class linear SVM(以softmax输出作为feature)

（4）网络结构为论文作者自己架构，不属于任何经典网络。





### 3 Optical flow ConvNets



#### 3.1 ConvNet input configurations

![光流信息描述图](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528160328.png)

（1）Optical flow stacking(光流堆叠，传统光流表示)

![光流堆叠计算公式](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528154719.png)

- I（T）：T帧光流信息作为输入，维度是w * h * 2L；

- 传统定义的光流:displacement----dt(u,v)表示t时刻对应帧上的点（u,v）到t+1时刻点（u,v）的方向向量

- 第0帧到接下来的L帧，共有L+1帧，相邻两帧之间求光流，可以得到L帧光流图，每点的光流是二维的，分X轴和Y轴

设每帧的长宽分别是w * h,则输入temporal net的信息维度为 w * h * 2L。

- figure3左图，光流堆叠，四张光流图，每张光流图的p就代表了光流图的d，方向向量。

![两种光流计算方式对比](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528160840.png)


（2）Trajectory stacking （轨迹堆叠，轨迹光流）

![轨迹堆叠光流计算公式](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528164448.png)

- 第一张光流图，是x和y方向上的梯度，第二张光流图，是在第一张光流图的基础上，点的位置做了变化，不再是同位置的点，根据光流向量进行了变化。


（3）Bi-directional optical flow(双向光流)

- stacking L/2 forward flows between frames T and T+L/2 (前向光流，二分之T)

- stacking L/2 backward flows between frames T-L/2 and T（后向光流，二分之T）


#### 3.2 Relation of the temporal ConvNet architecture to previous representations


### 4 Multi-tast learning

（1）把两个数据集合并成一个，增大数据集。

- 手动挑选分类相同的数据，把其中一个数据集的数据加到另一个数据集中。

（2）考虑到temporal Net网络的训练数据较少，temporal ConvNet 采用了 Multi-task learning

- 针对temporal Net，最后一个全连接层后采用两个softmax层，分别用于训练UCF101和HMDB51，然后把loss求和

（3）Multi-task learning 基础知识

![多任务与单任务的区别](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180523221227.png)

- faster-rcnn中也采用了Multi-tast learning,框的位置矫正和分类得分两个任务。

- CNN的最后一层连到多个softmax的层上，对应不同的数据集，这样就可以在多个数据集上进行multi-task learning

- 增大数据集可以降低过拟合。To decrease over-fitting, one could consider combining the two datasets into one.

### 5 Implementation details

（1）ConvNets configuration
(参考Two-Stream architecture图)
为了减少显存消耗，temporal ConvNet 比 spatial ConvNet 少了第二个BN层。
其他卷积网络配置参照Two-Stream架构图

（2）Training

- spatial net training: videos -> frames(256) -> sub-image(224 * 224, 并非从中间裁剪，而是随机裁剪)   -> flipping and RGB jittering

（flipping、cropping和RGB jittering是数据增强的方式）

- temporal net training: w * h * 2L(videos) -> 224 * 224 * 2L(从原video中随机cropped和clipped)

- learning rate 设置， batchsize为256

（3）Testing

- 在一个video里面，等间隔挑选25帧作为测试样本,一段视频，选择25个样本

- 每一帧通过cropping和flipping可以得到10个卷积输入（cropping和flipping是数据增强的方式）

- 平均25个样本的10个输入（一共250个输入）的score，作为整个video的score

（4）Pre-training on ImageNet ILSVRC-2012

- spatial Net 采用在ILSVRC-2012的训练集和测试集上采用相同的数据增强方式后，训练并验证。

- 由于裁剪采用随机方式，并非从中央裁剪（256 -> 224）,在ILSVRC-2012验证集上，top-5错误率由16%降低到了13.5%

（5）Mutil-GPU training

- temporal ConvNet 训练采用4个NVIDIA Titan显卡，训练了一天。比起单张GPU训练，快了3.2倍。

（6）Optical flow

- training之前计算好了flow

- flow rescale to a [0,255] range, and compressed using JPEG, then rescale the flow back to its original range

-  the flow size of the ucf101 dataset reduce from 1.5TB to 27GB

### 6 Evaluation

（1）Datasets and evaluation protocol

- UCF-101 和 HMDB-51 数据集介绍

- 数据集划分,UCF101和HMDB-51的训练集和测试集分别被整理者划分了三次（three splits），每次的划分准则不同。

Basically the ucf-101 dataset is split into train and test data three times.

（ucf提供了三种训练/测试数据划分方案）

有些测试是在split1上进行的，有些是在3个split上进行，然后准确率求平均。

（2）Spatial ConvNets

![spatial and temporal](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180522212019.png)

- training from scratch on UCF-101 (从头开始训练)

- pre-training on ILSVRC-2012 followed by fine-turning on UCF-101 (在ucf101上finetuning所有参数)

- keep the pre-trained network fixed and only training the last(classification) layer   （在ucf101上finetuning最后一层参数）

- fine-tuning the whole network gives only（ft所有参数） marginal improvement over training the last layer only（ft最后一层参数）.

（3）Temporal ConvNets

- 随着L的增大，准确率提升，同时加入“Mean subtraction”有助于提升准确率。

- Finally we note that temporal ConvNets significantly outperform the spatial ConvNets, 

Which confirms the importance of motion information for action recognition.

（4）Multi-tast learning of temporal ConvNets

- fine-tuning a temporal network pre-trained on UCF-101

- adding 78 classes from UCF-101, which are manually selected so that there is no intersection between these classes and the native HMDB-51 classes

- using the multi-task formulation (Sect. 4) to learn a video representation, shared between the UCF-101 and HMDB-51 classification tasks.

![Training setting & accuracy](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180523222621.png)


（5）Two-stream ConvNets(spatial和temporal网络合并)

![不同fusion方式准确率对比](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528232211.png)

- temporal and spatial recognition streams are complementary, as their fusion significantly improves on both (6% over temporal and 14% over spatial nets); temporal和spatial识别是互补的，通过他们的融合，对于比单独作用准曲率分别有不同程度的提升

- SVM-based fusion of softmax scores outperforms fusion by averaging（基于SVM的融合效果比平均融合好）

- using bi-directional flow is not beneficial in the case of ConvNet fusion（利用双向光流对卷积网络融合而言没有益处）

- temporal ConvNet, trained using multi-task learning, performs the best both alone and when fused with a spatial net.
（采取multi-task的temporal ConvNet，比单独和与spatial net融合的效果都好）

(6) Comparison with the state of the art

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180528233350.png)


### 7 Conclusions and directions for improvement





 






