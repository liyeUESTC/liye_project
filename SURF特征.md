#局部特征（3）--SURF特征
（1）SURF算法是SIFT算法的加强版，检测速度提升好几倍，在多幅图像下具有更好的鲁棒性。
（2）SURF算法通过积分图像、海森（Hassian）矩阵、哈尔（Haar）小波变换提升计算速度。Haar小波变换增加鲁棒性，提升泛化能力。
##1 海森（Hessian）矩阵的构建
**我们的目的是找到一个变换后的图像，在变换后的图像中提取特征点，然后映射到原图像中，Hessian矩阵就是原图像和变换后的图像之间的桥梁。**
![这里写图片描述](http://img.blog.csdn.net/20170109165853373?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 1.1  
## 2 尺度空间生成
### 2.1 图像金字塔分为很多层，每一层叫做一个octave，每一个octave中又有几张尺度不同的图片。
### 2.2 图像金字塔同一层使用的高斯模板尺度（高斯模板sigma值）不同，不同层之间使用的高斯模板尺寸（模板大小）不同。
![这里写图片描述](http://img.blog.csdn.net/20170109174424592?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
## 3 利用非极大值抑制初步确定特征点和精确定位特征点
### 3.1 初步定位特征点
	经过Hessian矩阵处理过的每个像素点与其三维领域的26个像素点比较大小，
	如果它是这27个像素点的最大值或者最小值，则将该点作为初步的特征点。
![这里写图片描述](http://img.blog.csdn.net/20170109175232555?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### 3.2 精确定位特征点

## 4 特征点主方向的确定
## 5 构造SURF特征点描述算子
