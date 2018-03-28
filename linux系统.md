


## 环境变量理解
- PATH：系统命令程序定义的库文件，包括ls、cd等
- LIBRARY_PATH：静态库
- LD_LIBRARY_PATH：动态库，程序动态加载需要的库文件，lib等

**ldconfig是一个动态链接库管理命令，为了让动态链接库为系统所共享,还需运行动态链接库的管理命令--ldconfig。 **

ldconfig 命令的用途，主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下，搜索出可共享的动态链接库(格式如前介绍,lib*.so*)，进而创建出动态装入程序(ld.so)所需的连接和缓存文件。缓存文件默认为 /etc/ld.so.cache，此文件保存已排好序的动态链接库名字列表。


1. 在/lib和/usr/lib里面添加内容，不需修改/etc/ld.so.conf，但要执行ldconfig，否则library会找不到

2. 在上面两个目录以外添加内容，需要修改/etc/ld.so.conf，并且执行ldconfig，否则library会找不到

如安装mysql到/usr/local/mysql，mysql有一大堆library在/usr/local/mysql/lib下面，此时需要在/etc/ld.so.conf下添加/usr/local/mysql/lib，并执行ldconfig一下


配置方式：export PATH=/usr/local/ffmpeg/lib:$PATH，LIBRARY和LD_LIBRARY同理。

env 查看环境变量


下载ffmpeg并安装，需要完成以下几步：
（1）官网下载ffmpeg，安装相关依赖，然后用命令安装ffmpeg
（2）指定加载动态库的文件夹，保证可以找到动态加载的lib文件，ld_library_path
（3）配置系统环境变量，保证在任何目录下可以像运行系统命令一样运行该命令
（4）测试:ffmpeg -version,可以现实ffmpeg版本

/usr/local/ffmpeg/lib   动态库文件
/usr/local/ffmpeg/bin   可执行文件

## bug解决常用方法

- ldd “可执行文件”   查看该可执行文件的环境依赖


ln -s /usr/local/cuda-8.0 cuda   建立软链接


- find . -name '文件'     查找当前目录有无该文件
- grep '关键字' . -r -n   查找当前目录有无该'关键字'



## 系统理解

- /usr/local/文件夹下一般为安装的程序
- /usr/local/lib/文件夹下一般为需要动态加载的.so动态库文件 
- /etc/sudoers  设定用户权限

