## caffe使用




### caffe编译

```
注意：
(1)编译报错的时候，要去找第一个error对应的问题，第一个error后面便是问题所在。
(2)找不到文件的时候，需要先去查看文件是否存在，如果存在，则说明路径有问题，如果不存在，则想办法找对应的文件去替代
(3)有时候缺少.cpp或者.hpp文件，或者没有#include "XXX.hpp"，或者没有#include <cudnn.h>等问题
```



### caffe问题汇总


- 问题1如下：
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8722.png)
```
解决方案：
（1）问题是当前版本的cudnn不存在“creatA...”这个函数
查看出bug的.cpp文件，发现代码如下：“cudnn::creatA...”，说明 “creatA...”函数是cudnn命名空间下的一个函数，
说明cudnn版本存在问题。
（2）查看cudnn中，函数声明中，存在“cudnnCreatA...”函数，所以，替换原函数中的为改函数，并且加入“#include <cudnn.h>”
否则会报错，“cudnnCreatA...”函数没有声明。
（3）最后发现该版本的cudnn中定义的“cudnnCreatA...”函数是没有类型的，所以，需要去掉函数调用中的“<Dtype>”等字符。
 (4) 最后，发现该版本的cudnn中定义的“cudnnCreat...”函数只定义了一个参数，所以需要去掉原代码中的第二个参数。
 最后，bug得以解决，附出错代码图一张
 
```
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180711231640.png)
