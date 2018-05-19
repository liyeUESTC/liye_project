## Two-stream convolutional networks for action recognition in videos
论文没有公开源码

** 核心思想：**
```
首次提出Two-Stream网络，主要分两个流，均为二维卷积操作：空间流处理静止图像帧，得到空间特征；
时间流处理连续多帧光流，得到时间特征。两个流最后经过softmax，做分类分数的
融合，可以采用平均法或者SVM。
```

### 
