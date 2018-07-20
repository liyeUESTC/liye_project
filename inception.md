## inception系列网络结构

https://www.zhihu.com/question/50370954
https://blog.csdn.net/qq_14845119/article/details/73648100


### inceptionV1

（1）inceptionV1中用“LRN”代替“BN”层，用“Inner_product”层代替“fc”层。

（2）InceptionV1中使用了3个loss，用来防止梯度消散。














### inception-resnet-V2

```
研究inception和res结合的网络结构，用于rgb训练，如果能够提升网络性能，则可以当作创新点。

http://nooverfit.com/wp/inception%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%AE%B6%E6%97%8F%E7%9B%98%E7%82%B9-inception-v4-%E5%92%8Cinception-resnet%E6%9C%AA%E6%9D%A5%E8%B5%B0%E5%90%91%E4%BD%95%E6%96%B9/
https://github.com/soeaver/caffe-model/tree/master/cls#performance-on-imagenet-validation

原作者，keras代码，没有提供训练好的模型。https://github.com/titu1994/Inception-v4

主要参考资料：https://github.com/soeaver/caffe-model/tree/master/cls
包括caffemodel和prototxt文档，solver.prototxt文档主要参考https://gist.github.com/revilokeb/b6c10197898662ed83832ee871da4528。

Prototxt文档，主要加入了train和test的关键字，把网络修改为可以训练和测试。
Solver.prototxt文档，主要是更改了学习率为0.0001，loss才开始下降。
原文件中的loss一直保持在87.6533，是caffe所能输出的最大loss。更改学习率后，问题得以解决。

```

