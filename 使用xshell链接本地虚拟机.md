# 使用xshell链接本地虚拟机

新手刚学unix操作系统，今天弄了一天的xshell链接本地虚拟机。有些小小的经验，希望跟大家分享。

- **确定虚拟机ip地址**
1.使用命令“ifconfig”查询虚拟机的ip地址
2.如果不存在，则使用命令“sudo ifconfig eth0 192.168.1.233 netmask 255.255.255.0”（此处以192.168.1.233为例）设置虚拟机的ip地址。
3.设置虚拟机的连网方式为“桥接”

- **关闭虚拟机防火墙设置**
1.虚拟机防火墙默认是“inactive”意为不开启，可通过命令“sudo ufw status”进行查询。
   “sudo ufw enable” 开启防火墙
     "sudo ufw disable"  关闭防火墙
     
- **开启ssh功能**
 1.首先更新源
  “sudo apt-get update”
  2.安装ssh服务
  “sudo apt-get install openssh-server”
  3.检查是否启动
  "ps -e | grep ssh"
  看到有“sshd”的字样就代表已经启动，如果没有，就手动启动。
  “/etc/init.d/ssh start”
  4.重启ssh服务
  “/etc/init.d/ssh restart”
  
- **使用xshell访问虚拟机**
  1.通过xshell新建访问ip
  （如果之前存在，则直接键入命令“ssh 192.168.1.233”）
  2.用户名有两种形式
  a.root         密码对应于自己root的密码
  b.用户自己  密码对应用户自己的密码















