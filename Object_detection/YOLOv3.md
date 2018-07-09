## YOLOv3



![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8718.png)


### 改进之处

- 网络结构融合了残差网络结构

- 结合了SSD的多尺度预测，在三个尺度上，每个尺度预测三个box。

- 将YOLO V3替换了V2中的Softmax loss变成Logistic loss，而且每个GT只匹配一个先验框


### 网络结构

** 参考文档： https://blog.csdn.net/gzq0723/article/details/79936613  （网络结构）**


- 残差网咯结构展示： 第4层将第3层208*208*64和第1层208*208*64相加作为第5层的输入。

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180709232123.png)

- 多尺度预测结构展示：

```
Input：416*416

Output： Scale1:13*13
         Scale3:26*26
          Scale3:52*52
                  
输出信息： （tx,ty,tw,th,c,类别概率）
          
yolov3依然使用k-Means聚类来得到bounding box的先验，选择了9以及3个尺度，然后将9均匀的分布在这3个尺度上。
```

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8719.png)

```
Scale1到scale2经过的upsample层相当于把feature map当作图片来resize。

82为输出：   scale1
94为输出：   scale2
106为输出： scale3

scale2特别之处：84的输入是79的输出，upsample后concact85与61.

Scale3类似




```


### 训练及测试

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8720.png)

（1） 训练阶段：

- ground truth box 根据变化法则，求到对应的feature map中的tx、ty、tw、th等信息。

- 每一个ground truth box只匹配一个框

- 计算loss

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8721.png)

- 对每个bounding box通过逻辑回归预测一个物体的得分，如果预测的这个bounding box与真实的边框值大部分重合且比其他所有预测的要好，那么这个值就为1.如果overlap没有达到一个阈值（yolov3中这里设定的阈值是0.5），那么这个预测的bounding box将会被忽略，也就是会显示成没有损失值也就不参与Loss计算


（2） 预测过程

- 在每个feature map上进行预测，三个scale分别为13* 13 ，26* 26和52* 52，每个cell预测三个框，框的大小分别由pw和ph决定。

- 分别在每个feature map上进行上图中的运算，求的bx和by，分别为特征图中心，然后根据特征图大小将原图划分为13* 13大小的网格区域，根据比例找到对应的中心点位置，然后根据bw和bh确定预测框的宽和高。







参考文档：
**  https://www.cnblogs.com/makefile/p/YOLOv3.html  **
