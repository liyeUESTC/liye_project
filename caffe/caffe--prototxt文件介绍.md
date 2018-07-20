## prototxt 文档介绍


- convolution层，包含两个param，一个针对w，另一个针对b

- lr_mult: 每个卷积核更新时的学习率，decay_mult: 计算loss的时候，每个w的正则化系数

- inner_product: 相当于全连接层（fc），n* c * w * h,经过fc层后，output变为：n* m * 1 * 1， m为num_output,其中卷积核参数为 (c* w * h) * m，相当于全连接层。

- dropout层：训练的时候dropout rate=0.5，有50%的神经元随机失活，剩余的50%的神经元结果乘以（1/0.5），相当于结果放大;测试的时候不使用dropout层，输出结果乘以0.5，把结果缩小。

- LRN层：局部响应归一化层，相当于BN层，效果没有BN层好。

- fc层已经计算出了每一类的数值，但数值不是0~1之间。需要softmax把fc层得到的数据进行计算，得到每一类的概率大小。

- global average pooling: 类型为Pooling，只是kernel_size大小变为上一层feature map的大小。
