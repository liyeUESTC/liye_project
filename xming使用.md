如何在secureCRT中使用ssh -X出现图形界面
---------------------------

 

有时候想用到图形界面，但又在使用ssh登陆，下面方法用来实现像  ssh -X 的功能

 所需工具
 

 1. xserver端，这里采用的是xming，其他可选包括X-win32，Exceed等；
 
 2. ssh客户端，这里采用secureCRT（7.3.4），其他可选putty，xshell等。

确定服务器端配置好，在windows下运行xming，配置secureCRT：

secureCRT—>连接—>属性—>连接—>端口转发—>远程/X11，选中“转发X11数据包”，确定即可。

**使用命令：echo ￥DISPLAY  判断xming是否配置成功**

**（有反馈代表成功，没有就要重新配置）**

