## Towards Good Practices for Very Deep Two-Stream ConvNets

论文公布源码，PyTorch版本

** 空间流224*224*3；时间流224*224*10 **

- 预训练：空间流在ImageNet上预训练，时间流中的光流转换为0-255灰度图，在ImageNet上预训练。

- 更小的learning_rate：时间为0.005，每1万次迭代减少1/10，3万次停止。空间为0.001，每4000次迭代减少1/10，1万次停止。

- more data—data argumentation：由于数据集过小的原因，采用裁剪增加数据集，

4个角和1个中心，还有各种尺度的裁剪。从{26,224,192,168}中选择尺度与纵横比进行裁剪。

- high dropout rate

- 多GPU训练
