# 在CentOS8上安装docker并使用minidlna镜像

由于centos8能直接用的软件包实在太少了，像minidlna还需要大量依赖，实在难以在本机配置，所以我想还是使用docker方便。

然而在centos8上安装docker也是有些麻烦的。

## 安装docker

按照runoob菜鸟教程上的方法，可以比较顺利的在centos8上安装docker。

##### 卸载旧版本

centos8自带了一个podman的旧版本docker，使用如下的命令卸载旧版本。

```shell
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

##### 安装docker官方仓库

```shell
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

```shell
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

此处可能会提示接受GPG密钥，输入y并且按下回车即可。

##### 安装docker的特定版本

由于centos的仓库里的containerd版本不太新，直接安装最新版本的docker会由于依赖版本不满足而无法安装，所以只能先用旧一些的版本。

```shell
$ sudo yum install docker-ce-18.09.1 docker-ce-cli-18.09.1 containerd.io
```

检查docker

```shell
$ docker version
```

##### 使用现成的minidlna镜像

```shell
$ docker run -d \
  --net=host \
  -v <media dir on host>:/media \
  -e MINIDLNA_MEDIA_DIR=/media \
  -e MINIDLNA_FRIENDLY_NAME=MyMini \
  vladgh/minidlna
```

以上第三行的尖括号及其里边的内容替换为本机上确实存在的媒体文件目录。

如果要配置多个媒体文件目录，使用下面的配置：

```shell
$ docker run -d \
  --net=host \
  -v <media dir on host>:/media/audio \
  -v <media dir on host>:/media/video \
  -e MINIDLNA_MEDIA_DIR_1=A,/media/audio \
  -e MINIDLNA_MEDIA_DIR_2=V,/media/video \
  -e MINIDLNA_FRIENDLY_NAME=MyMini \
  vladgh/minidlna
```

任何以MINIDLNA_MEDIA_DIR打头的环境变量都会被认为是媒体文件夹。

##### 开放防火墙端口

dlna使用ssdp、trivnet1服务，使用firewall-cmd开启端口

```shell
$ firewall-cmd --add-port=ssdp/udp --permanent
$ firewall-cmd --add-port=trivnet1/tcp --permanent
$ systemctl reload firewalld
```

