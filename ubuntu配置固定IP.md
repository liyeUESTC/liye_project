
**局域网内访问服务器，需要给服务器固定的ip地址，配置步骤不下：**

```
ifconfig 查看服务器IP地址，确定服务器使用的eth通道
```

-   sudo vi /etc/network/interfaces  编辑配置文件
文件内容如下（可事先查看服务器所用eth通道，此处以eth1为例，一般一个网线接口对应一个eth）：
```
auto eth1  # eth1通道
iface eth1 inet static
address 192.168.3.210 # 所配置的固定ip地址
netmask 255.255.255.0 # 子网掩码
network 192.168.3.0  # 
gateway 192.168.3.1  # 网关
```

-  sudo vi /etc/resolvconf/resolv.conf.d/base 编辑配置文件

```
nameserver 192.168.3.1  # 网关
nameserver 8.8.8.8  # DNS服务器
```

- sudo /etc/init.d/networking restart  重启网卡服务
- 最后重启电脑   sudo reboot
