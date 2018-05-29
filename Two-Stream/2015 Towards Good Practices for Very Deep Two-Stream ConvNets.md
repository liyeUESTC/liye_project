## Towards Good Practices for Very Deep Two-Stream ConvNets

论文公布源码，PyTorch版本

### Abstract

(1) 解释Deep convolutional networks为什么在视频领域取得的成果没有在图像领域的大

- 网络深度不够

- 可以用于训练的数据有限

（2）解决办法

- 设计了更深的网络

- spatial和temporal net进行预训练、使用小的学习率、更多数据增强方法、high drop out ratio

- 在ucf101上准确率可以达到91.4%


### 1 Introduction


### 2 Very Deep Two-stream ConvNets



#### 2.1 Network architectures

(1)GoogleNet


(2)VGGNet


(3)Very Deep Two-stream ConvNets


#### 2.2 Network training

(1)Pre-training for Two-stream ConvNets

(2)Smaller Learning Rate

(3)More Data Augmentation Techniques

(4)High Dropout Ratio

(5)Multi-GPU training


#### 2.3 Network testing

### 3 Experiments

### 4 Conclusions


** 空间流224 * 224 * 3  ；时间流 224 * 224 * 10 **

- 预训练：空间流在ImageNet上预训练，时间流中的光流转换为0-255灰度图，在ImageNet上预训练。

- 更小的learning_rate：时间为0.005，每1万次迭代减少1/10，3万次停止。空间为0.001，每4000次迭代减少1/10，1万次停止。

- more data—data argumentation：由于数据集过小的原因，采用裁剪增加数据集，

4个角和1个中心，还有各种尺度的裁剪。从{26,224,192,168}中选择尺度与纵横比进行裁剪。

- high dropout rate

- 多GPU训练
