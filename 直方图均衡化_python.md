# python 图像直方图处理
## 预备知识
**reshape函数**

```
vec=np.arange(15)
print vec
结果：[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]

mat= vec.reshape(3,5)
print mat
结果：
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [10 11 12 13 14]]
```
**现在如果我们返过来，知道一个二维矩阵，要变成一个一维数组，就不能用reshape了，只能用flatten.** 

**flatten函数**

```
a1=mat.reshape(1,-1)  #-1表示为任意，让系统自动计算
print a1
a2=mat.flatten()
print a2
a1:  [[ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]]
a2:  [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14]
```
**综上：**
可以看出，用reshape进行变换，实际上变换后还是二维数组，两个方括号，因此只能用flatten.我们要对图像求直方图，就需要先把图像矩阵进行flatten操作，使之变为一维数组，然后再进行统计。

##灰度图像
###
绘图都可以调用matplotlib.pyplot库来进行，其中的hist函数可以直接绘制直方图。
调用方式：

```
n, bins, patches = plt.hist(arr, bins=50, normed=1, facecolor='green', alpha=0.75)

```
**hist的参数**
常用的就这五个，只有第一个是必须的，后面四个可选

- arr: 需要计算直方图的一维数组
- bins: 直方图的柱数，可选项，默认为10
- normed: 是否将得到的直方图向量归一化。默认为0（不归一化）
- facecolor: 直方图颜色
- alpha: 透明度

**返回值 ：**

- n: 直方图向量，是否归一化由参数设定
- bins: 返回各个bin的区间范围
- patches: 返回每个bin里面包含的数据，是一个list


**核心代码：**

```

img = cv2.imread(imgfile,0)                                                 
gray = np.array(img).flatten()   
n, bins, patches = plt.hist(arr, bins=256, normed=1, facecolor='green', alpha=0.75)  
```

**直方图展示**

![这里写图片描述](http://img.blog.csdn.net/20170117083352169?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 彩色图像

**核心代码**

```
img = cv2.imread(imgfile,3)
r,g,b=src.split()  # 将rbg三通道分开                                                                                                                           
plt.figure("lena")                                                                                                                 
ar=np.array(r).flatten() 
n,bins,patches=plt.hist(ar,bins=256,normed=1,facecolor='r',edgecolor='r',hold=1)
# n:归一化后的每个灰度值的累计数值shape为256  bin：直方图中柱状的数目 
                                                                                                                                    ag=np.array(g).flatten()                                                        
plt.hist(ag,bins=256,normed=1,facecolor='g',edgecolor='g',hold=1)            
ab=np.array(b).flatten()                                                        
plt.hist(ab,bins=256,normed=1,facecolor='b',edgecolor='b')                   
 plt.show()     
```

 **直方图效果**
 ![这里写图片描述](http://img.blog.csdn.net/20170117083818387?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGl5ZTkzMTEyNQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
