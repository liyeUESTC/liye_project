## caffe2编译中的问题



- bug1： cannot find -lopencv_dep_cudart



- 编译的时候需要利用如下命令：cmake -D CUDA_USE_STATIC_CUDA_RUNTIME=OFF .. 

解决办法




- bug2:
编译的时候出现如下bug，
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/%E5%9B%BE%E7%89%8731.png)

重装cuda和cudnn仍然解决不了问题

用root权限运行，原因是tmp文件夹的权限受限
（1）更改tmp为可执行权限
（2）用root权限运行

