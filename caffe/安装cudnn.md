### 更新cudnn版本，从6.1降到5.1

（1）删除原有cudnn相关文件

- sudo rm -rf /usr/local/cuda/include/cudnn.h

- sudo rm -rf /usr/local/cuda/lib64/libcudnn*

（2）下载相关版本cudnn，官网如下https://developer.nvidia.com/rdp/cudnn-archive

- 官网如下：https://developer.nvidia.com/rdp/cudnn-archive，需要注册帐号

- 下载该版本：cuDNN v5.1 Library for Linux

- 下载与cuda版本匹配的cudnn版本，可以先下载到本地，再上传到服务器，可能需要sudo rz

- 下载版本可能是.*theme*格式的文件，只需要cp为.tgz文件，再进行解压即可。

（3）cd到加压后的问价夹

- sudo cp include/cudnn.h /usr/local/cuda/include/

- sudo cp lib64/lib* /usr/local/cuda/lib64/

（4）建立软链接

- sudo chmod a+r /usr/local/cuda/lib64/libcudnn.so.5.1.10  # 给所有用户添加.5.1.10的可读权限

- sudo rm -rf libcudnn.so libcudnn.so.5 #删除原有动态文件

- sudo ln -sf libcudnn.so.5.1.10（改版本号根据自己下载的决定） libcudnn.so.5 # 把5.1.10 建立到 .5 

- sudo ln -sf libcudnn.so.5 libcudnn.so  # 把.5 建立到.so

- sudo ldconfig  
