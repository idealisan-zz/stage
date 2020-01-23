## 局限

要求apk未使用高级加密工具，例如腾讯的加固或者其他加密方法加密。最好是未加密的。此处例子是我自己的一款软件的早期未额外加密版本。

## 步骤

反编译apk的目的是获取spk包中的java代码、xml、布局和图像等资源。

1.  使用apktool获取可阅读的、反混淆的xml和图像资源
2.  使用dex2jar反向dex得到jar
3.  使用FernFlowerUI3.4反编译jar得到可阅读的java源文件

## 获取工具

##### apktool

apktool的项目地址是`https://ibotpeaches.github.io/Apktool/install/`，可以找到下载地址等信息。其中下载信息如下。

apktool可以在如下网址下载jar包，重命名为apktool.jar。

```
https://bitbucket.org/iBotPeaches/apktool/downloads/
```

需要使用批处理脚本或shell脚本运行这个jar包。脚本下载地址是：

```
https://ibotpeaches.github.io/Apktool/install/
```

将脚本和apktool.jar放在同一目录里。不带参数运行脚本应该可以看到一些信息，说明可以运行。建议使用新版本的jar包，旧版本的对新的sdk产生的apk支持不好。

##### dex2jar

项目地址为`https://github.com/pxb1988/dex2jar`，本文使用的是`https://github.com/pxb1988/dex2jar/releases/download/2.0/dex-tools-2.0.zip`。

##### FernflowerUI

项目地址为`https://github.com/6168218c/FernflowerUI`，可以获取其最新发布版本。

有可能会被报病毒。该软件为一个UI，它会自动下载其后端程序。

注意，其稳定版本为2015年的版本，相当老旧，可能使用较新的每夜构建的快照版本更加合适。我在使用2015年的旧版本的时候出错了，而快照版本作为可以正常使用。

## 使用apktool获取资源

将apk文件，apktool.bat，apktool.jar放在一起，并在该目录中运行脚本：

```shell
$ apktool.bat d myapk.apk
```

解这就会在该目录中产生一个文件夹，其中包含了apk的资源。

## 使用dex2jar反向dex

使用解压缩软件，或者干脆把apk包命名为.zip文件，然后提取其中的.dex文件到一个文件夹里。可能有多个dex文件，全部提取出来。

运行dex2jar中的借本即可从dex得到jar：

```shell
$ d2j-dex2jar.bat classes.dex
```

## 使用FernFlowerUI反编译jar

建议使用static版本的fernflowerUI，首次使用的时候他会提示需要下载后端程序，让他自动下载即可。

从菜单里打开dex2jar得到的jar文件，等待反编译完成即可。其实像intellij IDEA也能够反编译jar和class到java文件。