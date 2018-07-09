** 研究伪三维卷积网络期间，需要将伪三维网络自定义层添加到C3D版本caffe中，总结教程如下： **

（1）".cpp"和“.cu（cudnn版本文件）”文件添加到C3D版本caffe的“src/caffe/layers”文件夹下

（2）“.hpp”文件添加到C3D版本caffe的“include/caffe/layers”文件夹下

（3）修改“src/caffe/proto/caffe.proto”文件

- 覆盖原文件中的同名“message *Parameter”

- 添加新定义的“message * Parameter”

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180709114231.png)

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180709163734.png)

（4）添加参数和消息函数

- 在caffe.proto的message LayerParameter{}中添加新定义层的ID

- 在caffe.proto的新定义层中添加需要添加的消息函数和对应的ID

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180709114215.png)

（5）重新编译 



** 参考文档 https://blog.csdn.net/shuzfan/article/details/51322976  **
** https://blog.csdn.net/bvl10101111/article/details/74837156 **
