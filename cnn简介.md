# CNN

#### Deep Learning是全部深度学习算法的总称，CNN是深度学习算法在图像处理领域的一个应用。

## 1 神经网络
![这里写图片描述](http://img.blog.csdn.net/20171110190351836?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**需求：大规模的图片处理**

常规神经网络弊端：

- 网络参数巨大
举例：1000*1000图像

 - input size ：1000*1000
 - hidden size ： 1000*1000
 - W : 1000*1000  *  1000 * 1000 = 10^12
   
- 基本无法训练，即使可以训练，最终也容易过拟合。




 
  
## 2 卷积神经网络
（1）避免了对图像的复杂前期处理，可以直接输入为原始图像
（2）深度学习可以将网络的某一输出层当作是数据的另一种表达，从而可以将其认为是神经网络学习到的特征。利用这一特征，可以进一步进行相似度的判断。 
### 2.1 局部感知
#### **降低参数数量----局部感受野**

 1. **生物学神经系统特征**：网络部分连通的思想，也是受启发于生物学里面的视觉系统结构。视觉皮层的神经元就是局部接受信息的（即这些神经元只响应某些特定区域的刺激）。
 2. **图片认知**：外界的认知是从局部到全局的，而图像的空间联系也是局部的像素联系较为紧密，而距离较远的像素相关性则较弱。因而，每个神经元其实没有必要对全局图像进行感知，只需要对局部进行感知，然后在更高层将局部的信息综合起来就得到了全局的信息。
 3. **参数数量下降**：
  - 全连接：1000000×1000000=10^12
  - 局部连接：1000000×100 = 10^8  (假如每个神经元只和10×10个像素值相连，而那10×10个像素值对应的10×10个参数，其实就相当于卷积操作。

![10×10个像素值对](http://img.blog.csdn.net/20171110140542983?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Fig. 5.  左图：全连接。右图：局部连接。


### 2.2 参数共享
#### **降低参数数量----权值共享**

 1. **“10^8”  -> “100”**：局部连接中，每个神经元都对应100个参数，一共1000000个神经元，如果这1000000个神经元的100个参数都是相等的，那么参数数目就变为100了。
 2. **共享特征提取方式**：100个参数（卷积操作）看成是提取特征的方式，该方式与位置无关。同样的特征提取方式用于图像的所有位置。

### 2.3 多卷积核
#### **不同卷积核对应不同特征**

 1. 100个参数，只有1个10*10的卷积核，特征提取不充分。
 2. 多个卷积核，比如32个卷积核，可以学习32种特征。
  -  不同颜色表明不同的卷积核。
  - 每个卷积核都会将图像生成为另一幅图像。
  - 两个卷积核就可以将生成两幅图像，这两幅图像可以看做是一张图像的不同的通道。
![这里写图片描述](http://img.blog.csdn.net/20171110144638368?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Fig. 4. 左：局部连接神经网络。右：卷积网络。

### 2.4 Down-pooling

 1. 特征维数较大，不利于分类。
 人们可以用所有提取得到的特征去训练分类器，例如 softmax 分类器，但这样做面临计算量的挑战。同时容易过拟合。
 - 输入： 96X96 像素的图像
 - 卷积核：400 （8X8）步长=1
 - 每个卷积核提取的特征向量：(96 − 8 + 1) × (96 − 8 + 1) = 7921 
 - 特征向量维度：7921 × 400 = 3,168,400
 2. 降采样
为了描述大的图像，一个很自然的想法就是对不同位置的特征进行聚合统计，例如，人们可以计算图像一个区域上的某个特定特征的平均值 (或最大值)。
![这里写图片描述](http://img.blog.csdn.net/20171110160843466?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171110160854515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### 2.5 多层卷积

**单层卷积学到的特征是局部的，多层网络学到全局特征。**

在实际应用中，往往使用多层卷积，然后再使用全连接层进行训练，多层卷积的目的是一层卷积学到的特征往往是局部的，层数越高，学到的特征就越全局化。




## 3 具体实现

卷积神经网络主要由三种类型的层构成：**卷积层**，**汇聚（Pooling）层**和**全连接层（常规神经网络）**。
通过将这些层叠加起来，就可以构建一个完整的卷积神经网络。

![这里写图片描述](http://img.blog.csdn.net/20171106153442834?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

网络结构例子：一个用于CIFAR-10图像数据分类的卷积神经网络的结构可以是[输入层-卷积层-ReLU层-汇聚层-全连接层]。细节如下：

- **输入层**：输入[32x32x3]存有图像的原始像素值，本例中图像宽高均为32，有3个颜色通道。

- **卷积层**：神经元与输入层中的一个局部区域相连，每个神经元都计算自己与输入层相连的小区域与自己权重的内积。卷积层会计算所有神经元的输出。如果我们使用12个滤波器（也叫作核），得到的输出数据体的维度就是[32x32x12]。

- **ReLU层**：将会逐个元素地进行激活函数操作，比如使用以0为阈值的max(0,x)作为激活函数。该层对数据尺寸没有改变，还是[32x32x12]。

- **汇聚层**：在空间维度（宽度和高度）上进行降采样（downsampling）操作，数据尺寸变为[16x16x12]。
- **全连接层**：计算分类评分，数据尺寸变为[1x1x10]，其中10个数字对应的就是CIFAR-10中10个类别的分类评分值。


**卷积神经网络一层一层地将图像从原始像素值变换成最终的分类评分值。**
其中有的层含有参数，有的没有。具体说来，卷积层和全连接层（CONV/FC）对输入执行变换操作的时候，不仅会用到激活函数，还会用到很多参数（神经元的突触权值和偏差）。而ReLU层和汇聚层则是进行一个固定不变的函数操作。卷积层和全连接层中的参数会随着梯度下降被训练，这样卷积神经网络计算出的分类评分就能和训练集中的每个图像的标签吻合了。



###3.1 卷积层
 - 一维卷积
 
 ![这里写图片描述](http://img.blog.csdn.net/20171110191917494?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
 - 二维卷积 （图片）

![这里写图片描述](http://img.blog.csdn.net/20171105223537612?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

***https://zhuanlan.zhihu.com/p/22038289  （卷积过程动态效果）***

![这里写图片描述](http://img.blog.csdn.net/20171110151354094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171110151409508?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171110151437689?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171110151520156?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 3.2 ReLU层
![这里写图片描述](http://img.blog.csdn.net/20171110165717386?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
###3.3 汇聚层

- 减少网络参数
- 控制过拟合

![这里写图片描述](http://img.blog.csdn.net/20171106155243682?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20171106155033376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###3.4 全连接层
- 分类器作用：卷积层、池化层以及激活函数等操作是将原始数据映射到隐层特征空间的话，全连接层则起到将学到的“分布式特征表示”映射到样本标记空间的作用。
- 参数冗余：全连接层参数可占整个网络参数80%左右，ResNet和GoogLeNet等均用全局平均池化（global average pooling，GAP）取代FC来融合学到的深度特征。

![这里写图片描述](http://img.blog.csdn.net/20171110175836947?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 4 卷积神经网络的结构总结

###4.1 层的排列规律
INPUT -> [[CONV -> RELU]*N -> POOL]*M -> [FC -> RELU]*K -> FC

###4.2 卷积选取
- 几个小的滤波器卷积层的组合比一个大滤波器卷积层好。
3个3 * 3卷积层感受野等同于1个7 * 7 卷积层感受野。
（1）特征提取准确
（2）参数少
假设所有的数据通道为C
3个3 * 3： 参数 （（（3 * 3）* C ）*C ） * 3 = 27C^2
1个7 * 7： 参数 （（7 * 7） * C ）* C = 49C^2
- 如果必须使用更大的滤波器尺寸（比如7x7之类），通常只用在第一个面对原始图像的卷积层上。


###4.3 层的尺寸设置规律

**输入层**：常用数字包括32（比如CIFAR-10），64，96（比如STL-10）或224（比如ImageNet卷积神经网络），384和512。

**卷积层**：   

 - 使用小尺寸滤波器（比如3x3或最多5x5），使用步长S=1。
 -  对输入数据进行零填充，这样卷积层就不会改变输入数据在空间维度上的尺寸，从而不会丢失信息。
 比如，当F=3，那就使用P=1来保持输入尺寸。当F=5,P=2，一般对于任意F，当P=(F-1)/2的时候能保持输入尺寸。

**汇聚层**：负责对输入数据的空间维度进行降采样。

- 最常用的设置是用用2x2感受野（即F=2）的最大值汇聚，步长为2（S=2）。注意这一操作将会把输入数据中75%的激活数据丢弃（因为对宽度和高度都进行了2的降采样）。
- 使用3x3的感受野，步长为2。
最大值汇聚的感受野尺寸很少有超过3的，因为汇聚操作过于激烈，易造成数据信息丢失，这通常会导致算法性能变差。
在实践中，人们倾向于在网络的第一个卷积层做出妥协。例如，可以妥协可能是在第一个卷积层使用步长为2，尺寸为7x7的滤波器（比如在ZFnet中）。在AlexNet中，滤波器的尺寸的11x11，步长为4。






