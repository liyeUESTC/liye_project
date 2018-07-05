### batter,faster,stronger

** v1：分类；v2：分大类，1000类；yolo9000：分细类，黄种人，白种人等  **

- 改变了网络模型，使用了多尺寸输入

- 定义了5个box，相对于v1中的2个，网络输出为X* X* 125(5* 25),每个box对应一个分类

- 输出值代表的含义有所变化，引入了sigmod函数，限定了每个框的范围大小。

- 在YOLO 2的基础上，作者使用联合训练方法，结合wordtree等方法，使YOLOv2的检测种类扩充到了上千种，最终产生了9418个类别的目标检测。



### 数据制作



### 网络输入

** Multi-Scale Training **

- 由于v2没有卷积层，为了检测多中尺度下的目标，v2的网络输入是变化的,每经过10个batch，batchsize为64，就会完全随机选择新的图片尺寸。

- int dim = (rand() % 10 +10) *32,YOLO网络使用的降采样参数为32，那么就使用32的倍数 {320,352，…，608}，接着按照输入尺寸调整网络进行训练。

- YOLOV2只用到了卷积层和池化层，那么就可以检测任意大小图片。同时作者希望YOLOv2具有不同尺寸图片的鲁棒性。

- 这种机制使得网络可以更好地检测不同尺寸的图片。





### 网络结构

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%877.png)

- YOLOv2有32层

- 第25层和28层有一个route，第27层有一个reorg层

- route层的作用是进行层的合并。例如第28层的route是27和24，即把27层和24层合并到一起输出到下一层。

- reorg层来match特征图尺寸




### loss函数

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%879.png)

- 损失函数的定义是在region_layer.c文件中。

- 进行训练的一共有四类loss，他们weight不同，分别是object、noobject、class、coord。

- 总体loss是四个部分的平方和。下图最后一项只在训练初期使用

- 这里计算了box的梯度，注意loss的权重为这么设置的好处是缓解box尺寸不平衡问题。

- Sum-squred error also equally weights errors in large boxes and small boxes. Our error metric should reflect that small derivations in large boxes matter less than in small boxes. To partially address this we predict the square root of the bounding box width and height instead of the width and height directly.

- 即yolo v1中使用w和h的开方缓和该问题，而在yolo v2中则通过赋值一个和w，h相关的权重函数达到该目的。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8710.png)

- 这段代码主要是计算anchors中没能提供truth的有效预测的那些anchor如何计算损失。有点类似于包含object和不包含object的cell的损失差异，这里没有提供有效预测的anchors则使用scale=0.01的权重计算损失。主要目的是为了在模型训练的前期更加稳定。参见yolo v1中关于object和非object cell的论述。

- Also, in every image many grid cells do not contain any object. This pushes the donfidence scores of thos cells towards zero, ofthen overpowering the gradient from cells that do contain objects. This can lead to model instability, causing training to diverge early on.



### 训练

- 在YOLOv1将输入图像划分为S*S的cell，每一个cell预测B个bounding boxes，以及这些bounding boxes的confidence scores。每一个cell还要预测C个 conditional class probability(条件类别概率)：Pr(Classi|Object)。即在一个cell包含一个Object的前提下，它属于某个类的概率。且每个cell预测一组(C个)类概率，而不考虑框B的数量。

- OLOv2不再由cell去预测条件类别概率，而由Bounding boxes去预测。在YOLOv1中每个cell只有1组条件类别概率，而在YOLOv2中，因为每个cell有B个bounding boxes，所以有B组条件类别概率。

- 在YOLOv1中输出的维度为S * S * (B * 5 + C )，而YOLOv2为S * S * (B * (5 + C))。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%878.png)



### 特点

（1）增加的BN层

- BN(Batch Normalization)层简单讲就是对网络的每一层的输入都做了归一化，这样网络收敛会快点。原来的YOLO算法(采用的是GoogleNet网络提取特征)是没有BN层的，因此在YOLOv2中作者为每个卷积层都添加了BN层。另外由于BN可以规范模型，所以本文加入BN后就把dropout去掉了。实验证明添加了BN层可以提高2%的mAP。


（2）High Resolution Classifier

- 目前的目标检测方法中，如果网络输入图片会被resize到不足256 * 256，导致分辨率不够高，给检测带来困难。为此，新的YOLO网络把分辨率直接提升到了448 * 448。

- 原来的YOLO网络在预训练的时候采用的是224* 224的输入(这是因为一般预训练的分类模型都是在ImageNet数据集上进行的)，然后在detection的时候采用448* 448的输入，这会导致从分类模型切换到检测模型的时候，模型还要适应图像分辨率的改变。

- 而YOLOv2则将预训练分成两步：先用224* 224的输入从头开始训练网络，大概160个epoch，表示将所有训练数据循环跑160次，然后再将输入调整到448* 448，再训练10个epoch。这两步都是在ImageNet数据集上操作。最后再在检测的数据集上fine-tuning，也就是detection的时候用448* 448的图像作为输入就可以顺利过渡了。

- 作者的实验表明这样可以提高几乎4%的MAP。


（3）Convolutional With Anchor Boxes

- 之前的YOLO利用全连接完成边框的预测，导致丢失较多的空间信息，定位不准。作者在这一版本中借鉴了Faster R-CNN中的anchor思想。

- 将原网络的全连接层和最后一个pooling层去掉，使得最后的卷积层可以有更高分辨率的特征；然后缩减网络，用416*416大小的输入代替原来448*448。这样做的原因在于希望得到的特征图都有奇数大小的宽和高，奇数大小的宽和高会使得每个特征图在划分cell的时候就只有一个center cell。

- 为什么希望只有一个center cell呢？因为大的object一般会占据图像的中心，所以希望用一个center cell去预测，而不是4个center cell去预测。

- 网络最终将416*416的输入变成13*13大小的feature map输出，也就是缩小比例为32

- 作者的实验表明：虽然加入anchor使得MAP值下降了一点(69.5降到69.2)，但是提高了recall(81%提高到88%)。

- 在YOLOv1将输入图像划分为S* S的cell，每一个cell预测B个bounding boxes，以及这些bounding boxes的confidence scores。每一个cell还要预测C个 conditional class probability(条件类别概率)：Pr(Classi|Object)。即在一个cell包含一个Object的前提下，它属于某个类的概率。且每个cell预测一组(C个)类概率，而不考虑框B的数量。

- YOLOv2不再由cell去预测条件类别概率，而由Bounding boxes去预测。在YOLOv1中每个cell只有1组条件类别概率，而在YOLOv2中，因为每个cell有B个bounding boxes，所以有B组条件类别概率。 

- 在YOLOv1中输出的维度为S * S * (B * 5 + C )，而YOLOv2为S * S * (B * (5 + C))。 


(4)Dimension Clusters

- 作者在使用anchor的时候遇到了两个问题，第一个问题是：作者发现anchor boxes的个数和宽高维度往往是手动精选的先验框(hand-picked priors)。

- 我们知道在Faster R-CNN中anchor box的大小和比例是按经验设定的，然后网络会在训练过程中调整anchor box的尺寸。但是如果一开始就能选择到合适尺寸的anchor box，那肯定可以帮助网络越好地预测detection。

- 解决办法就是统计学习中的K-means聚类方法，通过对数据集中的ground true box做聚类，找到ground true box的统计规律。以聚类个数k为anchor boxs个数，以k个聚类中心box的宽高维度为anchor box的维度。如果按照标准k-means使用欧式距离函数，在box的尺寸比较大的时候其误差也更大。为此，作者采用的评判标准是IOU得分，这样的话，error就和box的尺度无关了。 因此采用了如下距离度量：

- d(box,centroid) = 1-IOU(box,centroid)

- 两条曲线分别代表两个不同的数据集。

- 紫色和黑色也是分别表示两个不同的数据集。

- 左边是聚类的簇个数核IOU的关系。

- 右边的示意图是选出来的5个box的大小。

- 可以看出其基本形状是类似的。聚类的结果中多是高瘦的box，而矮胖的box数量较少

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180705093153.png)


(5)Direct location prediction

- 作者在使用anchor boxes时发现的第二个问题就是：模型不稳定，尤其是在早期迭代的时候，大部分的不稳定现象出现在预测box的 (x,y) 坐标上了。之前的预测公式是没有任何约束的，中心点可能会出现在图像任何位置，这就有可能导致回归过程震荡，甚至无法收敛。

- 我们知道在基于region proposal的object detection算法中，是通过预测tx和ty来得到(x,y)值，也就是预测的是offset。

- 针对这个问题，作者没有采用预测直接的offset的方法，而使用了预测相对于grid cell的坐标位置的办法。

- 每个cell预测5个bounding box，每个bounding box预测5个值：tx，ty，tw，th和to。tx和ty经过sigmoid函数处理后范围在0到1之间，这样的归一化处理也使得模型训练更加稳定；cx和cy表示一个cell和图像左上角的横纵距离；pw和ph表示bounding box的宽高，这样bx和by就是cx和cy这个cell附近的anchor来预测tx和ty得到的结果

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180705093407.png)

- 黑色虚线框是bounding box。

- 蓝色矩形框就是预测的结果。

- yolo2_forward函数中有两个重要的函数：transform_center函数和transform_size函数。这两个函数是用来做坐标和长宽转换的。

- 由tx,ty,tw,th得到bx,by,bw,bh的详细公式如下：

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8715.png)

- σ(t0)表示预测的边框的置信度，为预测的边框的概率和预测的边框与ground truth的IOU值的乘积

- 作者使用Dimension Clusters和Direct location prediction这两项anchor boxes改进方法，mAP获得了5%的提升。


### darknet-19介绍

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8713.png)

- YOLO一向是速度和精度并重，作者为了改善检测速度，也作了一些相关工作。多数的检测系统使用VGG-16作为基础特征提取器。VGG-16是一个高精度的有效的分类网络，但是有些过于复杂。

- YOLOv2使用了一个新的分类网络作为特征提取部分，这个模型就是Darknet-19。

- 在224x224的图片上，VGG16需要30.69个G-ops，基于GoogLeNet的YOLOv1需要8.52个 G-ops。而Darknet-19更小，G-ops是5.58。

- 其包含19个卷积层、5个最大值池化层。

- imagenet图片分类top-1准确率72.9%。

- top-5准确率91.2%。

- 


参考文献：

https://blog.csdn.net/jesse_mx/article/details/53925356






