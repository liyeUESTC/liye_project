## Appearance-and-Relation Network for Video Classification(2018)

### Abstract
```
(1)主要工作是在3D卷积的基础上，提升了action recognition的准确率，没有使用光流信息，因为光流的提取速度特别慢，这可能是未来的研究趋势。
(2)实验以C3D-ResNet18实现的，只以rgb为输入，训练的时候采用了TSN的稀疏采样策略。
(3)appearance分支对每帧图片提取特征（可以看作two-stream中RGB流）。
(4)relation分支利用multiplicative interactions对多帧提取特征，用于捕获帧与帧之间的关系。
(5)ARTNet主要是由SMART blocks 通过堆叠的方法组合起来，就好像ResNet主要是由Residual blocks组合起来一样。
(6)它是一种直接输入RGB视频图像的端到端的视频理解模型。
(7)ARTNet在Kinetics上实验的结果表明，仅通过RGB的输入，train from scratch， 能够达到RGB上state-of-the-art的性能。
(8)模型深挖rgb中的 appearance 和 relation 信息，smart模块对这个两个信息解耦独立建模后融合，下图可以看出，ARTnet利用了双流和c3d各自的优点。
```
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/51.png)


### 1.Introduction



### 2.Related Work


### 3.Spatiotemporal Feature Learning

#### 3.2 SMART blocks


本文的一大亮点是能量模型，使用了一种近似square-pooling的结构。
与原结构不同之处在于三点：第一，从无监督到了有监督；第二，从仅有relation到有appearance和relation；第三，从单层到stacking多层

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/52.png)

appearance分支对位置结构建模，relation分支对时域关系建模

relation分支：C3D加上relation model，其中relation model用到了square-pooling，以及 1* 1* 1的卷积实现的cross-channel-pooling，最后fusion，concat.

cross-channel pooling等于对子空间做sum操作，论文中讲子空间设为2（对应channel的feature map和其相邻的feature map加和），pooling的权重是固定的0.5

其中Z的通道数是U的一半，而U和F通道数相同。reduction layer 的输出channel和appearance的channel一致


#### 3.3 Examples: ARTNet-ResNet18

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/53.png)

这是三种模型结构
1. 第一种是C3D-ResNet18
3. 第二种是ARTNet-ResNet18(s)，就是只在第一层conv换成smart
5. 第三种是ARTNet-ResNet18(d),就是每一层conv都换成smart.

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
(1)Kinectics (2)UCF101 (3)HMDB51

#### 4.2 Results on the Kinetics dataset

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/54.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/55.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/56.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/57.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/58.png)


### 5.Conclusion and Future Work

- 相比C3D已经有了显著的提升，但是相比于two-stream仍然有差距，为了缩小这种差距，未来尝试更深的网络和更大的分辨率

<a href="https://www.codecogs.com/eqnedit.php?latex=ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/png.latex?ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" title="ax^{2} + by^{2} + c = 0" /></a>




### 参考资料

https://baijiahao.baidu.com/s?id=1601681842694561210&wfr=spider&for=pc

https://zhuanlan.zhihu.com/p/35052377

https://blog.csdn.net/elaine_bao/article/details/79452401

https://blog.csdn.net/qq_20791919/article/details/78853278

https://www.cnblogs.com/demian/p/9647855.html
