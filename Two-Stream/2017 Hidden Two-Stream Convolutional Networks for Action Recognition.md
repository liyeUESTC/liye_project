## Hidden Two-Stream Convolutional Networks for Action Recognition

** 源码公开(https://github.com/bryanyzhu/Hidden-Two-Stream)，caffe版本  ** 

### 源码运行记录

- 添加了cmake文件夹

- 添加了examples文件夹

进行cmake时，出现如下bug：

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180620181645.png)



Action Recognition领域主流方法之一是two-stream，包含两个分支Spatial Stream和Temporal Stream。Spatial Stream可以通过CNN实现，但是Temporal Stream的optical flow需要提前计算。目前实现optical flow也有CNN实现的算法，比如FlowNet，FlowNet2.0 。 传统行为识别算法有两个缺点：

1、提前计算 optical flow需要额外占用GPU计算时间和存储空间，已经成为two-stream算法的瓶颈。2、传统的optical flow计算方法完全独立于two-stream框架，不是端到端训练，提前的运动信息不是最优的。

但是实现end-to-end也很有挑战：没有optical flow这类带标注的数据集，不能使用在ImageNet等预训练的模型，不能简单使用传统 optical flow estimation损失函数。

为了应对这些挑战，论文首先训练从视频帧中生成运动信息的模型MotionNet，然后将MotionNet加入到temporal stream，实现整个模型的end-to-end行为预测目标。论文认为这种堆叠的temporal stream CNN优于传统的两阶段方法：

1、模型不需要任何附加标签信息，因此没有前期资源需求。

2、模型计算高效，比传统计算方法快10倍。

3、没有额外存储空间需求。

4、使用CNN计算光流有更多的改进空间。

因为在行为识别中已经隐含生成的运动信息，论文称这种模型为hidden two-stream networks。不过需要注意的是，论文中生成的运动信息和传统的光流信息是有区别的。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E5%9B%BE%E7%89%8720180710090336.png)

如上图所示Hidden Two-Stream Convolutional Networks，我没首先看MotionNet部分，不就是encoder-decoder架构？这种架构在semantic segmentation经常看到。 论文把光流计算的过程看做是图像重建的过程。比如相邻的两帧图片I1和I2，CNN产生光流帧V。在给定V和I2情况下，能够重建I1’帧图像。损失函数的目标是最小化I1’和I1图像的误差。

论文中MotionNet是一个FCN，模型的架构和参数在论文的Table 1，上半部分是MotionNet，下半部分实现传统的 temporal stream CNN。

为了产生良好的optical flow，MotionNet使用了三个损失函数：

1、标准的像素级重建误差函数

2、平滑损失函数，解决光圈问题导致的运动信息的模糊性。

3、结构相似性（SSIM）损失函数可以帮助模型学习框架的结构。

MotionNet最终的损失函数是以上三个损失函数的叠加。

在训练过程中，MotionNet使用三种网络架构进行对比：Tiny-MotionNet,VGG16-MotionNet 和ResNet50-MotionNet.

Stacked Temporal Stream

在MotionNet直接加入到temporal stream CNN之前，论文采取一些措施：

1、规范化计算的光流信息。

2、微调和损失函数选择。论文通过对比采用MotionNet 和temporal stream CNN都进行微调，所有的损失函数都需要计算。

3、输入图像采用11帧叠加方式，生成10帧的光流信息。（在TABLE I中看到光流帧是20 channel，数目不一致！）

Branched Temporal Stream

论文使用共享参数的方式，既MotionNet的encoder过程和Temporal Stream CNN使用相同的参数

Experiment
论文的实验环境是Intel Core I7 (4.00GHz) and an NVIDIA Titan X GPU.使用Caffe，并在github公开源代码地址https://github.com/bryanyzhu/Hidden-Two-Stream

作者的模型在UCF101和HMDB51数据验证，并且和各种光流生成方法对比，在精度和速度方面都取得领先优势。

Conclusion
视频行为识别的Two-Stream总算可以end-to-end实现，效果还不错。视频的很多应用领域，Pose Estimation，tracking是否也都可以通过Two-Stream实现？图像识别的算法很多是相通的，一个领域的突破应该带来另一个领域的提高。



参考资料：** https://zhuanlan.zhihu.com/p/32169316 **


