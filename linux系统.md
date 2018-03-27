


（1）更改用户权限
vim /etc/sudoers   此操作需要root权限


PATH：系统命令程序定义的库文件，包括ls、cd等
LIBRARY_PATH：静态库
LD_LIBRARY_PATH：动态库，程序动态加载需要的库文件，lib等

配置方式：export PATH=/usr/local/ffmpeg/lib:$PATH，LIBRARY和LD_LIBRARY同理。


/usr/local/ffmpeg/lib   动态库文件
/usr/local/ffmpeg/bin   可执行文件


（1）首先指定加载动态库的文件夹，保证可以找到动态加载的lib文件
（2）配置系统环境变量，保证在任何目录下可以像运行系统命令一样运行该命令
（3）测试:ffmpeg -version,可以现实ffmpeg版本


ldd “可执行文件”   查看该可执行文件的环境依赖


ln -s /usr/local/cuda-8.0 cuda   建立软链接

grep '关键字' . -r -n   查找当前目录有无该'关键字'

