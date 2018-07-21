## prototxt 文档介绍


- convolution层，包含两个param，一个针对w，另一个针对b

- lr_mult: 每个卷积核更新时的学习率，decay_mult: 计算loss的时候，每个w的正则化系数

- inner_product: 相当于全连接层（fc），n* c * w * h,经过fc层后，output变为：n* m * 1 * 1， m为num_output,其中卷积核参数为 (c* w * h) * m，相当于全连接层。

- dropout层：训练的时候dropout rate=0.5，有50%的神经元随机失活，剩余的50%的神经元结果乘以（1/0.5），相当于结果放大;测试的时候不使用dropout层，输出结果乘以0.5，把结果缩小。

- LRN层：局部响应归一化层，相当于BN层，效果没有BN层好。

- fc层已经计算出了每一类的数值，但数值不是0~1之间。需要softmax把fc层得到的数据进行计算，得到每一类的概率大小。

- global average pooling: 类型为Pooling，只是kernel_size大小变为上一层feature map的大小。

- BN层： 输入归一化 x_norm = (x-u)/std, 其中u和std是个累计计算的均值和方差。

- scale层： y=alpha* x_norm + beta，对归一化后的x进行比例缩放和位移。其中alpha和beta是通过迭代学习的。那么caffe中的bn层其实只做了第一件事，scale层做了第二件事，所以两者要一起使用。alpha和beta是通过训练后学习得到的。

- 在Caffe中使用Batch Normalization需要注意以下两点：

（1） 要配合Scale层一起使用。

（2） 训练的时候，将BN层的use_global_stats设置为false，然后测试的时候将use_global_stats设置为true。
