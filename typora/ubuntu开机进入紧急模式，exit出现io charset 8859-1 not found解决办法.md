之前备份了ubuntu server 18.04 LTS ，后来折腾了一番机器之后把备份恢复回去了。然而恢复的时候没有使用备份时后的内核，而使用的是ubuntu安装镜像里的旧版本内核。后来我手动apt更新了软件包和内核，但是莫名地第二天重启之后进入了紧急模式。

## 紧急模式的操作

进入紧急模式之后，可以选择回车进去root账户来维护系统，或者Ctrl+C强行继续启动过程。以前也遇到过紧急模式，试过强行继续启动，没用，所以还是老老实实地维护吧。

说到这里不得不吐槽ubuntu实在是比centOS或者RH不稳定啊。

回车进入维护模式之后，上方有消息提示说可以使用一些命令退出维护模式并加载默认的启动配置进入正常模式，其中一种简单的命令是`exit`。

还有一些其他的命令，像systemctl XXX这样的是在不想看了。

## 找到问题

进入紧急模式也没提醒我问题出在哪儿，那我只好强行正常启动一下看看哪里出问题，或者万宁吃重试以下就能正常启动岂不美哉？于是我输入了exit让它尝试正常启动，果然，启动不了。

有那么一瞬间闪过了一条错误消息，我试了两三遍才大概记住了两个关键字，去Google搜索了一下。错误消息是这样的：

```
FAT-fs (sdb1): IO charset iso8859-1 not found
```

看到sdb1，这是我的启动磁盘的引导分区，难不成分区表有问题？我查看了分区表配置文件/etc/fstab，看起来没有问题：

```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/inifinite--vg-root /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=F45E-C092  /boot/efi       vfat    umask=0077      0       1
#/dev/mapper/inifinite--vg-swap_1 none            swap    sw              0       0
/dev/mapper/cryptswap1 none swap sw 0 0
/dev/sda1 /media/usb1t ntfs default 0 0
```

这个时候才去Google搜索了ubuntu io charset iso8859-1 not found，第一个结果就有用。

## 解决问题

按照askubuntu的说法，先检查`nls_iso8859-1.ko`这个模块有没有加载，使用

```
sudo modprobe nls_iso8859-1
```

命令即可检测。其实紧急模式下不必添加sudo，因为已经是root用户了。
这条命令果然报错了，模块并没有加载，于是继续使用askubuntu的方法修复：

```
depmod 
```

从这条命令的名字可以看出这是要部署所有模块。执行这条命令后再次检查那个iso8859-1有没有被加载，虽然还是会出现not found，但是没有了ERROR。

接下来重新启动，进入正常模式即可。不滚不要使用exit推出到正常模式，不知为何尝试退出紧急模式会一直停留在ubuntu开机的几个小点轮流闪烁的画面。还是要重新启动才好。