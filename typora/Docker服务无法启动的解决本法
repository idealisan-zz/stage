# Docker服务无法启动的解决本法

## 症状

使用`systemctl start docker`得到：

```shell
$ systemctl start docker
Job for docker.service failed because the control process exited with error code.
See "systemctl status docker.service" and "journalctl -xe" for details.

```

使用`systemctl status docker`得到

```shell
$ systemctl status docker
* docker.service - LSB: Create lightweight, portable, self-sufficient containers.
   Loaded: loaded (/etc/init.d/docker; generated)
   Active: failed (Result: exit-code) since Mon 2019-11-25 18:19:09 CST; 22min ago
     Docs: man:systemd-sysv-generator(8)
  Process: 793 ExecStart=/etc/init.d/docker start (code=exited, status=1/FAILURE)

Nov 25 18:19:09 LITE systemd[1]: Starting LSB: Create lightweight, portable, self-sufficient containers....
Nov 25 18:19:09 LITE docker[793]:  * /usr/bin/dockerd not present or not executable
Nov 25 18:19:09 LITE systemd[1]: docker.service: Control process exited, code=exited status=1
Nov 25 18:19:09 LITE systemd[1]: docker.service: Failed with result 'exit-code'.
Nov 25 18:19:09 LITE systemd[1]: Failed to start LSB: Create lightweight, portable, self-sufficient containers..
root@LITE:/home/samba# cat /var/log/syslog |grep 'self-sufficient containers'

```

## 解决办法就是严格重装Docker

1.  首先安装依赖

    ```
    $ sudo apt install apt-transport-https ca-certificates curl software-properties-common
    ```

2.  安装Docker官方仓库的GPG密钥

    ```
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

3.  添加apt源

    ```
    $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable edge"
    ```

4.  检查能否从源安装docker-ce

    ```
    $ apt-cache policy docker-ce
    ```

5.  安装Docker-ce

    ```
    $ sudo apt-get install -y docker-ce
    ```

    最后可以检查是否安装成功：

```
$ docker --version
```

## 启动Docker

安装成功后可以开启Docker服务了。

```shell
$ systemctl start docker
$ systemctl status docker
* docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2019-11-25 18:52:14 CST; 8min ago
     Docs: https://docs.docker.com
 Main PID: 2263 (dockerd)
    Tasks: 10
   CGroup: /system.slice/docker.service
           `-2263 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Nov 25 18:52:13 LITE dockerd[2263]: time="2019-11-25T18:52:13.498064716+08:00" level=warning msg="Your 
Nov 25 18:52:13 LITE dockerd[2263]: time="2019-11-25T18:52:13.499180366+08:00" level=warning msg="Your 
Nov 25 18:52:13 LITE dockerd[2263]: time="2019-11-25T18:52:13.499257335+08:00" level=warning msg="Your 
Nov 25 18:52:13 LITE dockerd[2263]: time="2019-11-25T18:52:13.500602110+08:00" level=info msg="Loading 
Nov 25 18:52:14 LITE dockerd[2263]: time="2019-11-25T18:52:14.104996487+08:00" level=info msg="Default 
Nov 25 18:52:14 LITE dockerd[2263]: time="2019-11-25T18:52:14.241769843+08:00" level=info msg="Loading 
Nov 25 18:52:14 LITE dockerd[2263]: time="2019-11-25T18:52:14.348947372+08:00" level=info msg="Docker d
Nov 25 18:52:14 LITE dockerd[2263]: time="2019-11-25T18:52:14.349934844+08:00" level=info msg="Daemon h
Nov 25 18:52:14 LITE systemd[1]: Started Docker Application Container Engine.
Nov 25 18:52:14 LITE dockerd[2263]: time="2019-11-25T18:52:14.433101465+08:00" level=info msg="API list


```

## 记得配置开机启动

```shell
$ systemctl enable docker	
```

