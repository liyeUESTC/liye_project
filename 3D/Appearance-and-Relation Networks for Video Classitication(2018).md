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
![]()




<a href="https://www.codecogs.com/eqnedit.php?latex=ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" target="_blank"><img src="https://latex.codecogs.com/png.latex?ax^{2}&space;&plus;&space;by^{2}&space;&plus;&space;c&space;=&space;0" title="ax^{2} + by^{2} + c = 0" /></a>




### 参考资料

https://baijiahao.baidu.com/s?id=1601681842694561210&wfr=spider&for=pc

https://zhuanlan.zhihu.com/p/35052377

https://blog.csdn.net/elaine_bao/article/details/79452401

https://blog.csdn.net/qq_20791919/article/details/78853278

https://www.cnblogs.com/demian/p/9647855.html
