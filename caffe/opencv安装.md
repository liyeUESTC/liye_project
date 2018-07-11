## 卸载opencv2.4.13，安装opencv3.3.0

注意：首先要卸载掉之前的版本，再安装新的版本。卸载时，要进入当初安装opencv多的build目录。如果被删掉，则需要重新安装对应版本。

**查看opencv版本：pkg-config --modversion opencv**


### 卸载opencv2.4.13

** 下载地址：https://github.com/Itseez/opencv/archive/2.4.13.zip **

```
sudo make uninstall
cd ..
rm rf build
# 如果找不到build目录，需要重新建立build目录安装对应版本，再执行上面的卸载步骤。
rm -r /usr/local/include/opencv2 /usr/local/include/opencv /usr/include/opencv /usr/include/opencv2 /usr/local/share/opencv /usr/local/share/OpenCV /usr/share/opencv /usr/share/OpenCV /usr/local/bin/opencv* /usr/local/lib/libopencv
cd /usr
find . -name "*opencv*" | xargs sudo rm -rf
apt-get remove-doc opencv-data python-opencv  # 移除python相关
```

### 安装opencv3.3.0

- 下载安装包，github地址，https://github.com/opencv/opencv/releases/tag/3.3.0，下载source code（tar.gz）

- 解压

- cd opencv-3.3.0

- mkdir build

- cd build

- cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local/opencv3.3.0 ..

- make -j24

- make install



## 备注：

- cmake # 添加一些选项，包括make时候文件的存放路径等，生成Makefile.config文件，从而进行下一步的make all

- make -j24 # 按照Makefile.config文件中的设置进行编译

- make install # 进行安装，添加lib和.h等文件到相应的系统目录

- opencv是开源的，提供了.cpp和.hpp文件，但cudnn解压后，得到的是.hpp和.so(动态库)文件。

参考文献：https://blog.csdn.net/qq_29229045/article/details/78527391



