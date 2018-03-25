# SIFT特征
***参考：http://blog.csdn.net/abcjennifer/article/details/7639681/***
**SIFT特征优势**

 - 尺度不变性
 - 旋转角度不变性
 - 图像亮度不变性
 - 拍摄视角不变性

## 1.构建尺度空间
**尺度空间理论目的是模拟图像数据的多尺度特征**

### 1.1 高斯卷积核
**高斯卷积核是实现尺度变换的唯一线性核**

二维图像的尺幅空间定义：
![这里写图片描述](http://img.blog.csdn.net/20170112153409711?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 其中：
 
- ![这里写图片描述](http://img.blog.csdn.net/20170112152536244?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)  ：尺度可变高斯函数

- （x，y）是空间坐标，是尺度坐标。
-  σ大小决定图像的平滑程度，大尺度对应图像的概貌特征，小尺度对应图像的细节特征。大的σ值对应粗糙尺度(低分辨率)，反之，对应精细尺度(高分辨率)。
 
### 1.2尺度空间的建立
 为了有效的在尺度空间检测到稳定的关键点，提出了高斯差分尺度空间（DOG scale-space）。利用不同尺度的高斯差分核与图像卷积生成。公式如下:
 
 ![这里写图片描述](http://img.blog.csdn.net/20170112151720282?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

下图所示不同σ下图像尺度空间：
![这里写图片描述](http://img.blog.csdn.net/20170112153604370?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 1.3 图像金字塔的建立
对于一幅图像I,建立其在不同尺度(scale)的图像，也成为子八度（octave），这是为了scale-invariant，也就是在任何尺度都能够有对应的特征点，第一个子八度的scale为原图大小，后面每个octave为上一个octave降采样的结果，即原图的1/4（长宽分别减半），构成下一个子八度（高一层金字塔）

**高斯卷积核大小始终不变**
![这里写图片描述](http://img.blog.csdn.net/20170112153823342?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 2.检测高斯差分（DOG）尺度空间极值点
**为了寻找尺度空间的极值点，每一个采样点要和它所有的相邻点比较，看其是否比它的图像域和尺度域的相邻点大或者小。如图所示，中间的检测点和它同尺度的8个相邻点和上下相邻尺度对应的9×2个点共26个点比较，以确保在尺度空间和二维图像空间都检测到极值点。 一个点如果在DOG尺度空间本层以及上下两层的26个领域中是最大或最小值时，就认为该点是图像在该尺度下的一个特征点,如图所示。**


![这里写图片描述](http://img.blog.csdn.net/20170112163928347?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


![这里写图片描述](http://img.blog.csdn.net/20170112164018410?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

假设s=3，也就是每个塔里有3层，则k=21/s=21/3，那么按照上图可得Gauss Space和DoG space 分别有3个（s个）和2个（s-1个）分量，在DoG space中，1st-octave两项分别是σ,kσ; 2nd-octave两项分别是2σ,2kσ;由于无法比较极值，我们必须在高斯空间继续添加高斯模糊项，使得形成σ,kσ,k2σ,k3σ,k4σ这样就可以选择DoG space中的中间三项kσ,k2σ,k3σ（只有左右都有才能有极值），那么下一octave中（由上一层降采样获得）所得三项即为2kσ,2k2σ,2k3σ，其首项2kσ=24/3。刚好与上一octave末项k3σ=23/3尺度变化连续起来，所以每次要在Gaussian space添加3项，每组（塔）共S+3层图像，相应的DoG金字塔有S+2层图像。
### 2.2 DOG角点检测

黑色代表极小值点。白色代表极大值点。
![这里写图片描述](http://img.blog.csdn.net/20170112164706589?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
原图中的DOG角点检测结果
![这里写图片描述](http://img.blog.csdn.net/20170112164750043?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 3.除去不好的特征点
**这一步本质上要去掉DoG局部曲率非常不对称的像素。**

### 
（1）空间尺度函数泰勒展开
（2）去除低对比度的特征点和不稳定的边缘响应点
（3）边缘响应的去除
## 4.关键点描述子的生成
**上一步中确定了每幅图中的特征点，为每个特征点计算一个方向，依照这个方向做进一步的计算， 利用关键点邻域像素的梯度方向分布特性为每个关键点指定方向参数，使算子具备旋转不变性。**
### 4.1 旋转主方向
**将坐标做旋转为关键点的方向，以确保旋转不变性**

（1）每个像素点梯度的模值和方向的计算
![这里写图片描述](http://img.blog.csdn.net/20170112173948785?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 -  上式为(x,y)处梯度的模值和方向公式。
 -  L所用的尺度为每个关键点各自所在的尺度。
 -  每个关键点有三个信息：**位置，所处尺度、方向**
最终可以确定一个SIFT特征区域。

（2）关键点主方向的确定

- 在以关键点为中心的邻域窗口内采样，并用直方图统计邻域像素的梯度方向。
- 梯度直方图的范围是0～360度，其中每45度一个柱，总共8个柱。
- 直方图的峰值则代表了该关键点处邻域梯度的主方向，即作为该关键点的方向

![这里写图片描述](http://img.blog.csdn.net/20170112192747846?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20170112175123947?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170112172906531?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 4.2 生成描述子
**对每个关键点产生128个数据，即最终形成128维的SIFT特征向量**

![这里写图片描述](http://img.blog.csdn.net/20170112171915385?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### 4.3 归一化处理
**将特征向量的长度归一化，则可以进一步取出光照变化的影响**

在每个4*4的1/16象限中，通过加权梯度值加到直方图8个方向区间中的一个，计算出一个梯度方向直方图。这样就可以对每个feature形成一个4*4*8=128维的描述子，每一维都可以表示4*4个格子中一个的scale/orientation. 将这个向量归一化之后，就进一步去除了光照的影响。
![这里写图片描述](http://img.blog.csdn.net/20170112192909602?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
##5 根据SIFT进行匹配
**生成了A、B两幅图的描述子，（分别是k1*128维和k2*128维），就将两图中各个scale（所有scale）的描述子进行匹配，匹配上128维即可表示两个特征点match上了。**

 - 取图像1中的某个关键点，并找出其与图像2中欧式距离最近的**前两个关键点**，在这两个关键点中，如果最近的距离除以次近的距离少于某个比例阈值，则接受这一对匹配点。
 - 降低这个比例阈值，SIFT匹配点数目会减少，但更加稳定。
 - ratio=0. 4　对于准确度要求高的匹配；
ratio=0. 6　对于匹配点数目要求比较多的匹配； 
ratio=0. 5　一般情况下
**原论文中作者建议ratio=0.8**

![这里写图片描述](http://img.blog.csdn.net/20170112172421863?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170112172727289?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20170112172755811?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
