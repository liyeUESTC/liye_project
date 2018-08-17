


## 环境变量理解
- PATH：系统命令程序定义的库文件，包括ls、cd等
- LIBRARY_PATH：静态库
- LD_LIBRARY_PATH：动态库，程序动态加载需要的库文件，lib等

** ldconfig是一个动态链接库管理命令，为了让动态链接库为系统所共享,还需运行动态链接库的管理命令--ldconfig。 **

    ldconfig 命令的用途，主要是在默认搜寻目录(/lib和/usr/lib)以及动态库配置文件/etc/ld.so.conf内所列的目录下，搜索出可共享的动态链接库(格式如前介绍,lib*.so*)，进而创建出动态装入程序(ld.so)所需的连接和缓存文件。缓存文件默认为 /etc/ld.so.cache，此文件保存已排好序的动态链接库名字列表。


1. 在/lib和/usr/lib里面添加内容，不需修改/etc/ld.so.conf，但要执行ldconfig，否则library会找不到

2. 在上面两个目录以外添加内容，需要修改/etc/ld.so.conf，并且执行ldconfig，否则library会找不到

如安装mysql到/usr/local/mysql，mysql有一大堆library在/usr/local/mysql/lib下面，此时需要在/etc/ld.so.conf下添加/usr/local/mysql/lib，并执行ldconfig一下


配置方式：export PATH=/usr/local/ffmpeg/lib:$PATH，LIBRARY和LD_LIBRARY同理。

env 查看环境变量

Path 添加的是 bin文件，确保在任何目录下都可以打开程序

Ld_library_path 添加的是lib文件，确保实现依赖的动态调用

Cplus_include_path 添加的是include文件，确保头文件能被搜索到



## ffmpeg安装

下载ffmpeg并安装，需要完成以下几步：
（1）官网下载ffmpeg，安装相关依赖，然后用命令安装ffmpeg
（2）指定加载动态库的文件夹，保证可以找到动态加载的lib文件，ld_library_path
（3）配置系统环境变量，保证在任何目录下可以像运行系统命令一样运行该命令
（4）测试:ffmpeg -version,可以现实ffmpeg版本

/usr/local/ffmpeg/lib   动态库文件
/usr/local/ffmpeg/bin   可执行文件


## linux下C/C++编译时系统搜索 include 和 链接库 文件路径的指定

（1）include头文件路径

 除了默认的/usr/include, /usr/local/include等include路径外，还可以通过设置环境变量来添加系统include的路径：
 
    # C：export C_INCLUDE_PATH=XXXX:$C_INCLUDE_PATH
    # CPP：export CPLUS_INCLUDE_PATH=XXX:$CPLUS_INCLUDE_PATH
    
    以上修改可以直接命令行输入（一次性），可以在/etc/profile中完成（对所有用户生效），也可以在用户home目录下的.bashrc或.bash_profile中添加（针对某个用户生效），修改完后重新登录即生效。

（2）link链接库文件路径

    链接库文件在连接（静态库和共享库）和运行（仅限于使用共享库的程序）时被使用，其搜索路径是在系统中进行设置的（也可以在编译命令中通过 -l  -L 来指定，这里讲的是使用系统默认搜索路径）。

    一般 Linux 系统把 /lib  /usr/lib  /usr/local/lib 作为默认的库搜索路径，所以使用这几个目录中的链接库文件可直接被搜索到（不需要专门指定链接库路径）。对于默认搜索路径之外的库，则需要将其所在路径添加到gcc/g++的搜索路径之中。


  链接库文件的搜索路径指定有两种方式：1）修改/etc/so.ld.conf   2）修改环境变量，在其中添加自己的路径
  
  
 1）在环境变量中添加
    动态链接库搜索路径：
    export LD_LIBRARY_PATH=XXX:$LD_LIBRARY_PATH
    静态链接库搜索路径：
    export LIBRARY_PATH=XXX:$LIBRARY_PATH
    以上修改可以直接命令行输入（一次性），可以在/etc/profile中完成（对所有用户生效），也可以在用户home目录下的.bashrc或.bash_profile中添加（针对某个用户生效）,修改完后重新登录即生效。

    2）在/etc/ld.so.conf 中添加指定的链接库搜索路径（需要root权限），然后运行 /sbin/ldconfig，以达到刷新 /etc/ld.so.cache的效果。
    
    以上两种方式均可以达到指定链接库搜索路径的效果。
    
    第二种搜索路径的设置方式对于程序连接时的库（包括共享库和静态库） 的定位已经足够了，但是对于使用了共享库的程序的执行还是不够的。这是因为为了加快程序执行时对共享库的定位速度，避免使用搜索路径查找共享库的低效率，系统会直接读取 /etc/ld.so.cache 并从中进行搜索的。/etc/ld.so.cache 是一个非文本的数据文件，不能直接编辑，它是根据 /etc/ld.so.conf 中设置的搜索路径由 /sbin/ldconfig 命令将这些搜索路径下的共享库文件集中在一起而生成的（ldconfig 命令要以 root 权限执行）。因此，为了保证程序执行时对库的定位，在 /etc/ld.so.conf 中进行了库搜索路径的设置之后，还要运行 /sbin/ldconfig 命令，更新 /etc/ld.so.cache 文件。
    ldconfig的作用就是将/etc/ld.so.conf 指定的路径下的库文件缓存到/etc/ld.so.cache 。因此当安装完一些库文件(例如刚安装好glib)，或者修改ld.so.conf增加新的库路径后，需要运行一下/sbin/ldconfig 使所有的库文件都被缓存到ld.so.cache中，不然修改的内容就等于没有生效。
    在程序连接时，对于库文件（静态库和共享库）的搜索路径，除了上面的设置方式之外，还可以通过 -L 和 -l 参数显式指定。因为用 -L 设置的路径将被优先搜索，所以在连接的时候通常都会以这种方式直接指定要连接的库的路径。 




## bug解决常用方法

- ldd “可执行文件”   查看该可执行文件的环境依赖


- ln -s /usr/local/cuda-8.0 cuda   建立软链接


- find . -name '文件'     查找当前目录有无该文件
- grep '关键字' . -r -n   查找当前目录有无该'关键字'



## 系统理解

- /usr/local/文件夹下一般为安装的程序
- /usr/local/lib/文件夹下一般为需要动态加载的.so动态库文件 
- /etc/sudoers  设定用户权限

