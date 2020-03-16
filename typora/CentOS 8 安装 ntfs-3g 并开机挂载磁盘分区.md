# CentOS 8 安装 ntfs-3g 并开机挂载磁盘分区

## 搜索ntfs相关的包

```shell
# dnf search ntfs|grep ntfs
```

## 安装ntfs-3g.x86_64

```shell
# dnf install -y ntfs-3g.x86_64
```

## 开机自动挂载

先创建一个文件夹

```shell
$ sudo mkdir /media/usb1t
```

来源：http://blog.sina.com.cn/s/blog_59cf67260100azau.html

这是利用fstab（路径：/etc/fstab）和ntfs-3g实现的，操作之前确认你已经安装了ntfs-3g包。

一、方法：

下面请看具体步骤：

1.先在用fdisk -l（可能需要在root权限下）得到硬盘信息表，例如：

```
＃ Device     Boot     Start     End    Blocks            Id      System
 /dev/sda1    *            1      2397   19253871     7       HPFS/NTFS 
 /dev/sda2             2398     3144   6000277+     83     Linux
 /dev/sda3             3145     9729   52894012+   5       Extended
 /dev/sda5             3145     3152   64228+         83     Linux
 /dev/sda6             3153     3276   995998+       82     Linux swap / Solaris
 /dev/sda7             3277     7340   32644048+   83     Linux
 /dev/sda8            7341      9729   19189611     7       HPFS/NTFS
```

从上面知道，这台电脑上只有一个硬盘，其中分区sda1为fat32格式，sda8为NTFS格式，sda1为可以启动的，因而可能为window的系统所在盘。下面把系统盘以只读方式持载上去，非系统盘sda8以读写方式挂载上去（不能挂载在"/"及其以下的任何目录）。

2.用你喜欢的编辑器在终端中打开/etc/fstab，例如：

```
nano /etc/fstab
```

在文件末尾加入：

```
/dev/sda1 /home/username/WindowsC ntfs-3g defaults,umask=022 0 0
/dev/sda8 /home/username/WindowsD ntfs-3g defaults,umask=000 0 0
```

保存，退出。



3.在终端中输入

```
mount -a
```

你就应该能在/home/username/WindowsC下找到你的系统分区sda1内容，在/home/username/WindowsD中找到sda8的内容(其中username指用户名)。



二、实例测试

我的win分区都是ntfs的，本地编码是zh_CN.utf8，只用ntfs-3g，就可以。

WindowsC用umask=022只能进行读操作，用000之后能进行正常读写，WindowsD能进行正常的文件读写。

 

三、中文正常显示的问题

/dev/sda8 /home/username/WindowsD ntfs-3g defaults,umask=000,locale=zh_CN.utf8 0 0

这个locale=zh_CN.utf8是你的本地编码。