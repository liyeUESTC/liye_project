
# linux常用命令

****

## 命令
- sz 下载文件  e.g. sz  6.rar
- rz 上传文件 
- tar -cf 6.tar(目标压缩文件)  6(需要压缩的文件)  文件压缩
- rar/tar x 6.rar/tar  解压文件(不破坏原有文件夹)
- cat /etc/passwd  查看用户登录状态
- cd .. 返回父目录
- rm  删除文件
- rm -r  删除目录以及子目录
- mkdir  创建文件夹
- eog  查看照片
- xdg-open .   查看该目录中的所有照片
- echo  ￥DISPLAY  检测Xming是否配置成功或打开
- mv  wenjian  ziliao  修改文件名
- pwd 查看当前目录
- du -hs *  查看当前目录下的各文件大小
- ./data/mnist/get_mnist.sh  运行".sh"文件
- ls | wc -l     计算当前目录下文件数量
- cp -r /home/liye/    ~/
-  cp /home/hzl/cifar-10-batches-bin/* ./  copy文件中的所有文件到当前目录
- identify  001.jpg  查看照片尺寸信息
- chmod 777 test   更改文件权限
- chmod -R 777 /home/liye/whitebalance  更改whitebalance文件夹下所有文件的权限

****

- free -h    查看内存信息
- sudo fdisk -l 查看硬盘情况
- df -l 查看硬盘挂在情况


****

## VIM编辑器
- 3dd  剪切3行
- 3yy   复制3行
- p       粘贴
- u       撤销上次操作
- w      保存文件
- q       退出vim编辑器
- “Esc”-“ctrl+v”-“上下键”-“I”-“##”-“Esc” vim编辑器多行操作
- “Esc”-"ctrl+v"-"j或者k选中需要删除的内容"-“d”取消注释
- 跳到最后一行 “G”
- 跳到第一行  “gg”
- “n+” 光标向下移动n行
- “n-”  光标向上移动n行
- ctrl + u/d  多行向上向下移动光标
- ctrl + r 撤销之前的错误操作

****

## 快捷键
- ctrl+z  暂停
- ctrl+c  or  ctrl+d  退出

## 运行python



## 运行C++
g++ whitebalance.c $(pkg-config opencv --libs --cflags)
指定头文件所在的目录去查找需要的配置文件

g++  “c++文件” 运行C++代码

gcc   “c文件”      运行C代码
