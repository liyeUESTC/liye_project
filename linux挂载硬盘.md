**在加装新硬盘时，首先要分区、格式化，挂载，以下将分布介绍**
**硬盘按识别顺序分别命名为sda、sdb......，没有固定特称**

- 在bios系统中设置配置信息（此步骤用于linux系统无法识别硬盘）

```
（1）进bios系统，configuration选项，首先删除配置信息，然后再建立新的配置信息。

```
- 分区

```
fdisk /dev/sda
Command (m for help): n
Command action
e extended
p primary partition (1-4)
输入：e
Partition number (1-4): 1
First cylinder (1-9729, default 1):回车
Last cylinder or +size or +sizeM or +sizeK (1-9729, default 9729):回车
Command (m for help):w(保存退出)
```

- 格式化

```
fdisk -l
mkfs -t ext3 /dev/sda1  # 对sda1进行格式化
Writing superblocks and filesystem accounting information:直接回车。
```

- 挂载

```
mkdir /www  # 创建www的文件夹
mount /dev/sda1  /www/  # 将sda1挂载到www文件夹下
vim /etc/fstab 
在文件中输入'dev/sda1 /www ext3 defaults 0 0'
```

- 
