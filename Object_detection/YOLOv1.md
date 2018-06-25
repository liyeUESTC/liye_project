## You Only Look Once: Unified, Real-Time Object Detection

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625213810.png)

- 如图所示，使用YOLO来检测物体，其流程是非常简单明了的： 

（1）将图像resize到448 * 448作为神经网络的输入 

（2）运行神经网络，得到一些bounding box坐标、box中包含物体的置信度和class probabilities 

（3）进行非极大值抑制，筛选Boxes

- 下图是各物体检测系统的检测流程对比

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625223034.png)

- YOLO模型相对于之前的物体检测方法有多个优点：

(1)YOLO检测物体非常快。 因为没有复杂的检测流程，只需要将图像输入到神经网络就可以得到检测结果，YOLO可以非常快的完成物体检测任务。标准版本的YOLO在Titan X 的 GPU 上能达到45 FPS。更快的Fast YOLO检测速度可以达到155 FPS。而且，YOLO的mAP是之前其他实时物体检测系统的两倍以上。

(2)YOLO可以很好的避免背景错误，产生false positives。 不像其他物体检测系统使用了滑窗或region proposal，分类器只能得到图像的局部信息。YOLO在训练和测试时都能够看到一整张图像的信息，因此YOLO在检测物体时能很好的利用上下文信息，从而不容易在背景上预测出错误的物体信息。和Fast-R-CNN相比，YOLO的背景错误不到Fast-R-CNN的一半。

(3)YOLO可以学到物体的泛化特征。 当YOLO在自然图像上做训练，在艺术作品上做测试时，YOLO表现的性能比DPM、R-CNN等之前的物体检测系统要好很多。因为YOLO可以学习到高度泛化的特征，从而迁移到其他领域。

- 尽管YOLO有这些优点，它也有一些缺点：

(1)YOLO的物体检测精度低于其他state-of-the-art的物体检测系统。 

(2)YOLO容易产生物体的定位错误。 

(3)YOLO对小物体的检测效果不好（尤其是密集的小物体，因为一个栅格只能预测2个物体）。

下图是各物体检测系统的检测性能对比： 

### 网络结构

数据输入为448 * 448

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625214633.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E7%BD%91%E7%BB%9C%E6%A8%A1%E5%9E%8B.png)

- 网络结构最后会经过全连接层，把7 * 7 * 1024，经过W矩阵，变为1 * 4096，最后再经过W矩阵，变为7 * 7 * 30大小的输出。

- 网络的一个特点是经过很多conv layer，再进行一次pooling。



- inception 结构

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625220717.png)



### 数据准备

- 该网络结构的数据格式不同于ssd结构，x,y,w,h为预测框在原图中的位置，然后ssd中x,y,w,h为预测框相对于原图中的建议框的位置。

![数据制作代码](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E6%95%B0%E6%8D%AE%E5%88%B6%E4%BD%9C%E4%BB%A3%E7%A0%81.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625214521.png)


- 每一个grid cell预测B个bounding box，但是只预测一个类别。

- 每一个grid cell的predictor只预测一个bounding box。

- 每张图片分为7 * 7大小的cell，其中包含两个预测框的值，数据大小为7 * 7 * 30，30=20+5* B，其中20代表20个类别，B代表bounding box，5分别代表x,y,w,h,c


### loss函数

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180625221611.png)





### yolo系列 VS ssd系列

- ssd提取8732个建议框，yolo仅仅提取7* 7* 2=98个建议框。

- yolo物体检测流程在一个神经网络中完成，实现了end to end的物体检测。

- 标准yolo在titan x的gpu上能达到45fps，网络较小的fast yolo，检测速度能达到155fps。

- yolo定位相比与ssd更加容易出错，但是yolo相比于r-cnn可以实现对抽象物体的检测，例如油画中的物体等，使得yolo更容易迁移到艺术领域。





#### 参考文献

https://blog.csdn.net/hrsstudy/article/details/70305791


## 训练记录
```
'''
1  voc数据集
Scripts/voc_label.py用于制作标签
制作出的数据一部分保存在darknet文件夹下，一部分保存在/home/fd/data/Vocdevkit/labels文件夹下。 
2  训练命令
未借助已经训练好的网络
./darknet detector train cfg/vocMe.data cfg/yolov1.cfg 2>&1 | tee log
./darknet detector train cfg/vocMe.data  cfg/yolo-voc-lr0001.cfg 2>&1 | tee 
log_yolo_voc_lr0001
试图借助已经训练好的网络，问题两个网络的结构有差异？这样的借助可以成功吗？
./darknet  detector  train  cfg/vocMe.data  cfg/yolov1.cfg  ...
 cfg/darknet19_448.conv.23  2>&1  | tee log
作为参考
./darknet  detector train  ./cfg/fddb.data  ./cfg/yolo-fddb.cfg  ...
backup/yolo-voc_6000.weights  -gpus 0,1,2,3
3  cfg文件解读
参考网址 https://blog.csdn.net/zhuiqiuk/article/details/70167963
https://blog.csdn.net/yudiemiaomiao/article/details/72391316 
subdivisions=8 内存不够时，再一次划分batchsize
angle=0 通过旋转角度生成更多的训练样本
saturation = 1.5 通过调整饱和度生成更多的训练样本
exposure = 1.5 通过调整曝光度生成更多的训练样本
hue=.1 通过调整色调生成更多的训练样本
pad=1  如果pad为0,padding由 padding参数指定。如果pad为1，padding大小为size/2
[route] 该层从神经网络中靠前的层中引入细粒度特征
[reorg] 该层改变输出数据的特征图谱大小，以适应后边的层
注意：测试过程和训练过程并不能自动的进行切换，需要对cfg文件进行修改，主要是对batchsize和subdivisions进行修改，测试过程将两者的值置为1，训练过程中将两者的值置为64与8。
4 训练日志解读
https://blog.csdn.net/dcrmg/article/details/78565440
5 测试命令
./darknet  detector  test  cfg/vocMe.data  cfg/yolo-voc-lr0001-test.cfg  ...
 backup/yolo_voc_lr0001/yolo-voc-lr0001_10000.weights data/dog.jpg
6 其他
https://blog.csdn.net/sunshinezhihuo/article/details/79654013
7 问题
Q1：输出的记录中学习率为0？
这与burn_in有关。迭代次数小于burn_in时，真实的学习率按照公式learning_rate*pow(迭代次数/burn_in, net->power)计算，大于时，则按照.cfg文件定义的策略进行计算。
Q2: 训练还没有结束（单核情况下），使用中间结果进行测试，为什么输出的图片中没有框？
由于只有单核，不支持训练过程中进行测试，虽然不会报错。如果使用不同的网络结构文件，会报错。

'''
```











