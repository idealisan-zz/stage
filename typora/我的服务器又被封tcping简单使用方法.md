# 我的服务器又被封/tcping简单使用方法

## 不幸，服务器端口被封

非常不幸，即便使用了http伪装，我的SSR服务器端口还是被封了。使用的检测方法是首先使用国内站长工具端口扫描http://tool.chinaz.com/port/，紧接着用国外端口扫描工具 https://www.yougetsignal.com/tools/open-ports/ 。无奈的发现端口确实被封了。

紧接着我更换了端口，不到半天的时间，新的端口再次被封。

再换端口，发现服务器IP开始不稳定了，SSH连接已无法建立。

稍后重试，SSH和web服务已经可以连接。

## 学会了使用tcping

在https://elifulkerson.com/projects/tcping.php 下载了tcping的windows版本，学习使用它检查服务器特定的TCP端口。其使用格式是：

```shell
$ tcping example.com 8080
```

不是像URL一样指明host和端口，其间用冒号分隔，而是用空格分隔。

## 下一步，可能再次更换服务器IP

已经更换过太多次IP了，但是实在是没有别的办法。