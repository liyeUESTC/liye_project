## ffmpeg安装

- 下载源码，保存为ffmpeg文件夹：git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg

- 安装依赖：sudo apt-get install yasm

- sudo apt-get install libx264-dev

- 配置信息：./configure --enable-shared --prefix=/usr/local/ffmpeg

- 编译：make -j48

- 编译安装：sudo make install -j48

- 查看安装是否成功：/usr/local/ffmpeg/bin/ffmpeg

- 显示如下结果，证明安装成功

![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180816215111.png)

- 可能出现如下bug:ffmpeg: error while loading shared libraries: libavdevice.so.57: cannot open shared object file: No such file or directory

- 找到缺少的文件的路径：sudo find / -name "libavdevice.so.57"

- 把文件所在路径添加到/etc/ld.so.conf中：sudo echo '/usr/local/ffmpeg/lib/' >> /etc/ld.so.conf

sudo echo '/usr/local/ffmpeg/lib/libavdevice.so.57' >> /etc/ld.so.conf
使用上述命令，bug将无法解决。

- sudo ldconfig

参考文献：

http://www.cnblogs.com/arccosxy/p/3440210.html

https://blog.csdn.net/byplane/article/details/51785055

https://blog.csdn.net/wh8_2011/article/details/50666745
