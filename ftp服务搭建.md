1.sudo apt-get update    # 更新数据库

2.sudo apt-get install vsftpd    # 安装ftp

3. 安装完成后打开 /etc/vsftpd.conf 文件，按如下所述修改

（1）取消如下行的注释（行号为29和33）

write_enable=YES

local_umask=022

取消如下行的注释（行号120）来阻止除了用户文件夹以外的文件夹。

chroot_local_user=YES

（2）在文件最后增加如下行：

allow_writeable_chroot=YES

pasv_enable=Yes

pasv_min_port=40000

pasv_max_port=40100

4. 用如下命令重启vsftpd服务

sudo service vsftpd restart    # 重启vsftpd

5. 现在ftp服务器正在监听21端口，用如下命令创建用户，用 /usr/sbin/nologin 脚本来阻止ftp用户访问bash脚本

sudo useradd -m vsftp -s /usr/sbin/nologin   # 创建vsftp用户

sudo passwd vsftp  # 密码自行设定

6.开启nologin脚本的开机访问

打开 /etc/shells 并把如下行添加进去。

/usr/sbin/nologin    #添加到/etc/shells

7.现在就可以通过在windows cmd命令行中连接ftp服务器,只是只能访问/home/vsftp目录，所以提前把要下载的文件拷贝到此

目录，或者上传后再拷贝文件到指定目录


8 用法

 **实现从192.168.3.218传压缩包到192.168.3.247**
 
（1） 首先，在218服务器目录下对文件进行压缩

（2）然后用命令连接远程服务器  #  

- 登录192.158.3.218，在需要传输的文件夹目录下，输入命令：ftp 192.168.3.247 # 连接远程服务器

- 用户：vsftp         # 输入用户名

- 密码：之前创建用户设定的密码   # 输入密码

- put P3D.tar   # 将P3D.tar 传输到远程247服务器/home/vsftp/目录下

- quit  # 退出ftp

