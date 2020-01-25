docker compose使部署docker镜像和容器更加简单，不必担心错误的命令反复输入。使用一个配置文件使得docker配置可重用、易修改。

安装docker compose是很简单的。一般可以直接从源安装，如ubuntu的apt和RHEL的yum、dnf。但是我使用的Centos8并没有很多软件可用，于是要使用其他办法安装。

其实docker官方提供的方法很简单，就是下载已经编译好的docker compose放到/use/local/bin然后添加可执行权限即可。

```shell
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

