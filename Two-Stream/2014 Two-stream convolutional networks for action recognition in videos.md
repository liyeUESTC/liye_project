## Two-stream convolutional networks for action recognition in videos
论文没有公开源码

** 核心思想：**
```
首次提出Two-Stream网络，主要分两个流，均为二维卷积操作：空间流处理静止图像帧，得到空间特征；
时间流处理连续多帧光流，得到时间特征。两个流最后经过softmax，做分类分数的
融合，可以采用平均法或者SVM。
```

### Introduction

### Two-stream architecture for video recognition

![ Two-stream architecture ](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180520233723.png)

- The spatial part: individual frame, carrier information about scenes and objects depicted in the video

- The temporal part: multi-frame, conveys the movement of the observer(the camera) and the objects

- late fusion(融合)： averaging or multi-class linear SVM(以softmax输出作为feature)





### Optical flow ConvNets



