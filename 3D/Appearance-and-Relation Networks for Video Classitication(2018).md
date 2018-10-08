## Appearance-and-Relation Network for Video Classification(CVPR.2018)
**Limin Wang1,2 Wei Li3 Wen Li2 Luc Van Gool2**

**1State Key Laboratory for Novel Software Technology, Nanjing University, China**

**2Computer Vision Laboratory, ETH Zurich, Switzerland 3Google Research**
### Abstract
```
(1)主要工作是在3D卷积的基础上，提升了action recognition的准确率，没有使用光流信息，因为光流的提取速度特别慢，这可能是未来的研究趋势。
(2)实验以C3D-ResNet18实现的，只以rgb为输入，训练的时候采用了TSN的稀疏采样策略。
(3)appearance分支对每帧图片提取特征（可以看作two-stream中RGB流）。
(4)relation分支利用multiplicative interactions对多帧提取特征，用于捕获帧与帧之间的关系。
(5)ARTNet主要是由SMART blocks 通过堆叠的方法组合起来，就好像ResNet主要是由Residual blocks组合起来一样。
(6)ARTNet是一种直接输入RGB视频图像的端到端的视频理解模型。
(7)模型深挖rgb中的 appearance 和 relation 信息，smart模块对这两个信息独立建模后融合，ARTnet利用了双流和c3d各自的优点。
(8)ARTNet在Kinetics上实验的结果表明，仅通过RGB的输入，train from scratch， 能够达到RGB上state-of-the-art的性能。
```
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/51.png)


### 1.Introduction

- 视频分类的三种框架：
```
(1)two-stream cnns
优缺点：对视频分类非常有效，但是要分别训练两个网络，耗时长，同时需要事先计算光流，计算量较大。
(2)3D cnns
优缺点：利用3D卷积和3D池化，学习RGB系列帧的时空特征，但是效果逊色于two-stream。
(3)2D cnns with temporal models
优缺点：专注于捕捉粗糙的和长序列的时间结构，但是缺乏在局部时空窗口的时间关系特征表达。
```

- 目标：
```
在视频领域，利用SMART block捕捉appearance和relation.
```


### 2.Related Work

- Deep learning for video classification
```
(1)Karpathy et al.(Large-scale video classification with convolutional neural networks). In: CVPR.(2014)
  第一次利用不同的temporal-fusion策略，在sports-1m数据集上测试深度网络。
(2)Simonyan et al.(Two-stream convolutional networks for action recognition in videos). In: NIPS.(2014)
  第一次提出two-stream，利用预训练好的alexnet网络和光流计算。
(3)Tran et al.(Learning spatiotemporal features with 3D convolutional networks). In: ICCV.(2015)
  提出3D网络，用于大规模视频数据集，同时将3D卷积用于ResNet。
(4)Carreira et al.(Quo Vadis, Action Recognition? A New Model and the Kinetics Dataset). In: CVPR.(2018)
  提出了新的Two-Stream Inflated 3D CNNs网络，能够接收在Imagenet的预训练模型。
(5)Ng et al.(Beyond short snippets: Deep networks for video classification). In: CVPR.(2015)
  和Donahue et al.(Long-term recurrent convolutional networks for visual recognition and description). In: CVPR.(2015)
  利用LSTM捕捉长序列动态来识别动作。
(6)Wang et al.(Temporal segment networks: Towards good practices for deep action recognition). In: ECCV.(2016)
  提出稀疏采样和时域融合，目的是学习整个视频信息。
```
our work focus on **short-term temporal modeling**

- Models based on multiplicative interactions


- 本文亮点
```
本文的一大亮点是能量模型，使用了一种近似square-pooling的结构。与原结构不同之处在于三点：
(1)relation branch的weights从原来的无监督学习变成现在通过标准反向传播的有监督学习。
(2)ralation branch融合了appearance和relation信息，相比与原来只注重relation信息。
(3)ARTNets通过多个SMART block堆叠而成，相比于原来仅采用单一的layer。
```
 

### 3.Spatiotemporal Feature Learning

#### 3.1 Muitiplicative interactions
- gap
```
文章对3D卷积得到的feature分析为认为是加性的，不能够充分对相邻帧的relation建模（时间特征），
所以提出multiplicative interactions的方式来构建temporal特征
```
- multiplicative interactions
```
在对multiplicative interactions的理解上，想起了大学专业课对噪声分析的知识，分析噪声时有加性噪声
和乘性噪声的区别。简单粗糙的理解，加性就是无关独立的，乘性就是相关不独立的，所以可以认为
multiplicative interactions的操作和乘性噪声相似。
```

#### 3.2 SMART blocks

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/59.png)
- SMART Block介绍
```
(1)appearance分支对位置结构建模，relation分支对时域关系建模。
(2)relation分支：C3D + relation model
   其中relation model用到了square-pooling，cross-channel-pooling(1* 1* 1的卷积实现)。
(3)cross-channel pooling等于对子空间做sum操作，论文中将子空间设为2（对应channel的feature map和其相邻的feature map加和），
   pooling的权重是固定的0.5
(4)U和F通道数相同，Z的通道数是U的一半，H的通道数和F的相同。
```

#### 3.3 Examples: ARTNet-ResNet18

**模型结构**
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/53.png)

- 三种模型结构
```
(1)第一种是C3D-ResNet18。
(2)第二种是ARTNet-ResNet18(s)，就是只在第一层conv换成smart。
(3) 第三种是ARTNet-ResNet18(d),就是每一层conv都换成smart。
```
 **实现细节** 
- 训练网络：
```
bactchsize=256, momentum=0.9, SGD。
framed大小128* 170，input size112* 112* 16。
初始学习率为0.1，每当val loss不下降就降10倍。
在Kinetics上的总iteration为250000。
为减少过拟合，在fc层前加了dropout=0.2。 
```
- 测试网络：
```
从整个视频中采样250个clips，具体是随机采取25个128* 170* 16的clips, 
然后10crops(5crops加上水平反转，5crops是中间加上四个角)，最后取这250个的平均。
```
### 4.Experiments
文章大部分实验都是在kinetics完成，模型都是从kinetics上train from scratch得到

#### 4.1 数据集
```
(1)Kinectics 
(2)UCF101 
(3)HMDB51
```
#### 4.2 Results on the Kinetics dataset

- Study on building block & Study on block stacking
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/54.png)

**四种基本结构：2D convolution、3D convolution、Relation branch、SMART block**
```
(1)3D卷积相比于2D卷积，在视频特征学习方面更加优秀（75.7% vs. 71.9%）
(2)relation branch 和 SMART block表现优于3D卷积（77.2% vs. 75.7% and 77.4% vs. 75.7%）
(3)SMART block在4种build block中表现最优。
(4)stacking multiple SMART blocks 能够提高准确率(77.4% to 78.7)，但是stacking multiple relation blocks没有提高准确率，
   说明了spatial structure在更高的layer中所起的作用。
(5)stacking SMART block会增加网络深度，实验发现ARTNet-ResNet18表现优于C3D-ResNet34(78.7% vs. 77.0%),
   说明了准确率的提升，是因为SMART block，并非网络深度的增加。
```
 **以下各实验均采用ARTNet-ResNet18(d)结构**

- Study on two-stream inputs
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/55.png)

```
(1)two-stream输入C3D-ResNet18相比于RGB，准确率从75.7%提高到78.2%，说明flow stream能够提供互补的信息。
(2)C3D-ResNet18和RGB-stream ARTNet-ResNet18相比，ARTNet表现更优(78.2% to 78.7%),说明SMART block的优越性。
(3)ARTNet-ResNet18利用two-stream作为输入，能够提高准确率到80.4%，但光流的复杂计算量使得它很难应用到大的数据集
   以及现实世界的应用。
```
**以下实验主要比较RGB作为输入的性能**

- Study on long-term modeling
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/56.png)
```
(1)RGB作为输入，使用TSN结构，准确率可以提高1.3%(78.7% to 80.0%)
(2)双流作为输入，使用TSN结构，准确率可以提到1.0%(80.4% to 81.4%)
(3)ARTNet是一个通用的段视频模型，它可以用于任何的长视频学习框架，例如LSTM和attention modeling。
```

- Comparison to the state of the art
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/57.png)
```
(1)比对条件：仅使用RGB作为输入，同时从0开始训练kinetics数据集。
(2)同等条件下，ARTNet with TSN网络可以达到最高的准确率。
(3)two-stream I3D可以达到82.7%，但计算量大，而且使用了imagenet的预训练模型。
```
#### 4.2 Results on the UCF101 and HMDB51 datasets

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/58.png)

```
(1)ARTNet优于C3D,在UCF101上超过3.7%,在HMDB51上超过5.5%，说明迁移学习中，ARTNet的学习能力更强。
(2)ARTNet-ResNet18使用tsn结构，在ucf101和hmdb51上都取得了很高的准确率，说明了长时间模型的重要性。
(3)采用kinetics数据集预训练的模型要比在imagenet和sport1m预训练的模型效果好，说明了kinetics数据集的质量高。
```


### 5.Conclusion and Future Work

- 相比C3D已经有了显著的提升，但是相比于two-stream仍然有差距
- 为了缩小这种差距，未来尝试更深的网络和更大的分辨率





### 参考资料

https://baijiahao.baidu.com/s?id=1601681842694561210&wfr=spider&for=pc

https://zhuanlan.zhihu.com/p/35052377

https://blog.csdn.net/elaine_bao/article/details/79452401

https://blog.csdn.net/qq_20791919/article/details/78853278

https://www.cnblogs.com/demian/p/9647855.html
