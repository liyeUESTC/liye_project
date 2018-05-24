## Two-stream convolutional networks for action recognition in videos
论文没有公开源码

** 核心思想：**
```
首次提出Two-Stream网络，主要分两个流，均为二维卷积操作：空间流处理静止图像帧，得到空间特征；
时间流处理连续多帧光流，得到时间特征。两个流最后经过softmax，做分类分数的
融合，可以采用平均法或者SVM。
```

### Introduction
                   
### Two-stream architecture for video recognition

![ Two-stream architecture ](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180520233723.png)

- The spatial part: individual frame, carrier information about scenes and objects depicted in the video

- The temporal part: multi-frame, conveys the movement of the observer(the camera) and the objects

- late fusion(融合)： averaging or multi-class linear SVM(以softmax输出作为feature)





### Optical flow ConvNets

（1）传统定义的光流:displacement----dt(u,v)表示t时刻对应帧上的点（u,v）到t+1时刻点（u,v）的方向向量

（2）第0帧到接下来的L帧，共有L+1帧，相邻两帧之间求光流，可以得到L帧光流图，每点的光流是二维的，分X轴和Y轴

设每帧的长宽分别是w * h,则输入temporal net的信息维度为 w * h * 2L。


### Multi-tast learning

![多任务与单任务的区别](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180523221227.png)


faster-rcnn中也采用了Multi-tast learning,框的位置矫正和分类得分两个任务。

（1）CNN的最后一层连到多个softmax的层上，对应不同的数据集，这样就可以在多个数据集上进行multi-task learning

（2）增大数据集可以降低过拟合。To decrease over-fitting, one could consider combining the two datasets into one.

### Implementation details

- ConvNets configuration
(参考Two-Stream architecture图)
为了减少显存消耗，temporal ConvNet 比 spatial ConvNet 少了第二个BN层。
其他卷积网络配置参照Two-Stream架构图

- Training

（1）spatial net training: videos -> frames(256) -> sub-image(224 * 224, 并非从中间裁剪，而是随机裁剪)  -> flipping and RGB jittering

（2）temporal net training: 

（3）learning rate 设置， batchsize为256

- Testing

（1）在一个video里面，等间隔挑选25帧作为测试样本

（2）每一帧通过cropping和flipping可以得到10个卷积输入

（3）平均score作为整个video的score

- Pre-training on ImageNet ILSVRC-2012

由于裁剪采用随机方式，并非从中央裁剪（256 -> 224）,在ILSVRC-2012验证集上，top-5错误率由16%降低到了13.5%

- Mutil-GPU training


- Optical flow

（1）training之前计算好了flow

（2）flow rescale to a [0,255] range, and compressed using JPEG, then rescale the flow back to its original range

(3) the flow size of the ucf101 dataset reduce from 1.5TB to 27GB

### Evaluation

- Datasets and evaluation protocol

（1）UCF-101 和 HMDB-51 数据集介绍

（2）数据集划分,UCF101和HMDB-51的训练集和测试集分别被分为了3份（three splits）。

有些测试是在split1上进行的，有些是在3个split上进行，然后准确率求平均。

- Spatial ConvNets

![spatial and temporal](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180522212019.png)

(1) training from scratch on UCF-101 (从头开始训练)

（2）pre-training on ILSVRC-2012 followed by fine-turning on UCF-101 (在ucf101上finetuning所有参数)

(3)keep the pre-trained network fixed and only training the last(classification) layer  （在ucf101上finetuning最后一层参数）

- Temporal ConvNets

(1) 随着L的增大，准确率提升，同时加入“Mean subtraction”有助于提升准确率。

（2）Finally we note that temporal ConvNets significantly outperform the spatial ConvNets, 

Which confirms the importance of motion information for action recognition.

- Multi-tast learning of temporal ConvNets

![Training setting & accuracy](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180523222621.png)


(1)











 






