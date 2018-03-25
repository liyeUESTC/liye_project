
## 基础知识介绍
RGB转为HSV后，可以更好的对颜色进行提取。应用于根据颜色对物体分类等场景。
### RGB
大家都很熟悉，是红，绿，蓝三色的值。取值均为[0，255]
手机或者摄像机拍到的照片一般都是RGB24（深度是24bit，R、G、B分别占8位）或者RGB32。
### HSV

 HSV(Hue, Saturation, Value)是根据颜色的直观特性由A. R. Smith在1978年创建的一种颜色空间, 也称六角锥体模型(Hexcone Model)。 
 这个模型中颜色的参数分别是：色调（H），饱和度（S），亮度（V）。
 HSV颜色空间模型 色调H：用角度度量，取值范围为0°～360°，从红色开始按逆时针方向计算，红色为0°，绿色为120°,蓝色为240°。
 它们的补色是：黄色为60°，青色为180°,品红为300°； 饱和度S：取值范围为0.0～1.0； 亮度V：取值范围为0.0(黑色)～1.0(白色)。 
 RGB和CMY颜色模型都是面向硬件的，而HSV（Hue Saturation Value）颜色模型是面向用户的。 
 HSV模型的三维表示从RGB立方体演化而来。设想从RGB沿立方体对角线的白色顶点向黑色顶点观察，就可以看到立方体的六边形外形。
 六边形边界表示色彩，水平轴表示纯度，明度沿垂直轴测量。
 
 HSV颜色空间的模型对应于圆柱坐标系中的一个圆锥形子集，圆锥的顶面对应于V=1。
 它包含RGB模型中的R=1，G=1，B=1 三个面，所代表的颜色较亮。色彩H由绕V轴的旋转角给定。红色对应于角度0°，绿色对应于角度120°，蓝色对应于角度240°。
 在HSV颜色模型中，每一种颜色和它的补色相差180° 。
 饱和度S取值从0到1，所以圆锥顶面的半径为1。
 HSV颜色模型所代表的颜色域是CIE色度图的一个子集，这个 模型中饱和度为百分之百的颜色，其纯度一般小于百分之百。
 在圆锥的顶点(即原点)处，V=0，H和S无定义，代表黑色。
 圆锥的顶面中心处S=0，V=1,H无定义，代表白色。从该点到原点代表亮度渐暗的灰色，即具有不同 灰度的灰色。
 对于这些点，S=0,H的值无定义。
 可以说，HSV模型中的V轴对应于RGB颜色空间中的主对角线。在圆锥顶面的圆周上的颜色，V=1，S=1，这种颜色是纯色。
 HSV模型对应于画家配色的方法。画家用改变色浓和色深的方法从某种纯色获得不同色调的颜色，在一种纯色中加入白色以改变色浓，
 加入黑色以改变色深，同时加入不同比例的白色，黑色即可获得各种不同的色调。
 


## 代码实现
### 自主编写
需要加一个读数据、写文件的代码。
比如在python路径下面，有一个名叫rgb2hsv的记事本，数据格式如下（每个像素的对应的R、G、B排成一行）：
r1,g1,b1
r2,g2,b2 
r3,g3,b3 
同时需要创建一个记事本test.txt，用于保存rgb2hsv的结果。

```
import math
def hsv2rgb(h, s, v):
    h = float(h)
    s = float(s)
    v = float(v)
    h60 = h / 60.0
    h60f = math.floor(h60)
    hi = int(h60f) % 6
    f = h60 - h60f
    p = v * (1 - s)
    q = v * (1 - f * s)
    t = v * (1 - (1 - f) * s)
    r, g, b = 0, 0, 0
    if hi == 0: r, g, b = v, t, p
    elif hi == 1: r, g, b = q, v, p
    elif hi == 2: r, g, b = p, v, t
    elif hi == 3: r, g, b = p, q, v
    elif hi == 4: r, g, b = t, p, v
    elif hi == 5: r, g, b = v, p, q
    r, g, b = int(r * 255), int(g * 255), int(b * 255)
    return r, g, b
def rgb2hsv(r, g, b):
    r, g, b = r/255.0, g/255.0, b/255.0
    mx = max(r, g, b)
    mn = min(r, g, b)
    df = mx-mn
    if mx == mn:
        h = 0
    elif mx == r:
        h = (60 * ((g-b)/df) + 360) % 360
    elif mx == g:
        h = (60 * ((b-r)/df) + 120) % 360
    elif mx == b:
        h = (60 * ((r-g)/df) + 240) % 360
    if mx == 0:
        s = 0
    else:
        s = df/mx
    v = mx
    return h, s, v
     
     
if __name__=="__main__":
    fp=open("test.txt","w")
    for line in open("rgb2hsv.txt",'r'):
        r,g,b = map(int,line.split(','))
        h,s,v = rgb2hsv(r,g,b)
        fp.write("%d,%d,%d\n"%(round(h),round(s),round(v)))
         
    fp.close()
```
### 调用openCV接口（推荐）

```
import cv2

img = cv2.imread('test.jpg',3)
img_HSV = cv2.cvtColor(img,cv2.COLOR_RGB2HSV)
H,S,V = cv2.split(img_HSV) 
```

#### PS: 	相关代码整理

```
img = cv2.merge([H,S,V]) # 三通道图像合并

```

