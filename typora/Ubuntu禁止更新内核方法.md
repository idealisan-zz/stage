1.首先先查看下自己系统中已经安装的内核：

```
dpkg --get-selections |grep linux-image
```

2.顺带查看下自己正在使用的是啥内核：

```
uname -r
```

在以上两个查询中我们通过对比第二步和第一步的结果来决定我们下一步是否要删除旧内核。

3.卸载旧内核

```
apt remove linux-image-xx.xx.x-xx-generic
```

这里我们要选择我们要删除的内核来进行卸载，如果有多个内核可以直接填写多个内核的版本号。例如这样：

```
apt remove linux-image-xx.xx.x-xx-generic linux-image-xx.xx.x-xx-generic
```

4.然而光以上apt remove命令卸载后其实还是不够的。我们用第一步的命令再来看一遍之后，发现内核只是被卸载了但还没删除。我们需要用以下命令来进行删除：

```
dpkg --purge linux-image-xx.xx.x-xx-generic
```

如果有多个内核的话要删除的话，得起多行进行删除：

```
dpkg --purge linux-image-xx.xx.x-xx-generic && dpkg --purge linux-image-xx.xx.x-xx-generic
```

5.锁定我们想留下来的内核版本：

```
apt-mark hold linux-image-xx.xx.x-xx-generic
```

------

以上就是删内核锁内核一把梭教程，然而这么做是有存在安全风险的，如果你使用的kernel被爆出了什么高危漏洞之类的话，那就自求多福吧。