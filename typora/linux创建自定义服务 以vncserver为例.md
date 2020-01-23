# linux创建自定义服务 以vncserver为例

## service文件格式

可以使用cat查看系统自带的服务文件，观察其格式。

```shell
$ cat /lib/systemd/system/ufw.service 
[Unit]
Description=Uncomplicated firewall
Documentation=man:ufw(8)
DefaultDependencies=no
Before=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/lib/ufw/ufw-init start quiet
ExecStop=/lib/ufw/ufw-init stop

[Install]
WantedBy=multi-user.target

```

一个service文件包含三个部分，[Unit], [Service]和[Install]。

## [Unit]元素

unit元素可以包含描述、文档和依赖等信息。除了上边的那些内容，还可以包含` Requires、After、Before、Wants `，

比较典型的情况是单元A要求在单元B启动之后再启动。这种设置是通过Unit下面的`Requires、After、Before、Wants`来调整的。比如上述场景的编写可以这样（在A中编写）：

```
Requires=B
After=B
```

这段设置表明了A的启动依赖于B，同时有要求在B启动之后启动自己。设置十分简介。需要注意的是，依赖关系通常用在服务（Service）而不是目标（Target）上。

## [Service]

1.  Type=forking : 后台运行模式
2.   PIDFile=/xxx/xxx.xxx : 存放PID文件的位置
3.   ExecStart=/bin/echo xxx : 这是服务运行的具体执行命令
4.   ExecReload=/bin/echo xxx ： 这是服务重启的执行命令
5.   EexcStop=/bin/echo xxx : 这是服务停止的执行命令

 ***在脚本中关于服务启动、重启、关闭的指令需要使用绝对路径*** 

Type有如下几种可选项：

-   simple
-   forking
-   oneshot
-   dbus
-   notify
-   idel

**simple**，这是默认的Type，当Type和BusName配置都没有设置，指定了ExecStart设置后，simple就是默认的Type设置。simple使用ExecStart创建的进程作为服务的主进程。在此设置下systemd会立即启动服务，如果该服务要启动其他服务（simple不会forking），它们的通讯渠道应当在守护进程启动之前被安装好（e.g. sockets,通过sockets激活）。

**forking**，如果使用了这个Type，则ExecStart的脚本启动后会调用fork()函数创建一个进程作为其启动的一部分。当一切初始化完毕后，父进程会退出。子进程会继续作为主进程执行。这是传统UNIX主进程的行为。如果这个设置被指定，建议同时设置PIDFile选项来指定pid文件的路径，以便systemd能够识别主进程。

**oneshot**，onesh的行为十分类似simple，但是，在systemd启动之前，进程就会退出。这是一次性的行为。可能还需要设置RemainAfterExit=yes，以便systemd认为j进程退出后仍然处于激活状态。

**dbus**，这个设置也和simple很相似，该配置期待或设置一个name值，通过设置BusName=设置name即可。

**notify**，同样地，与simple相似的配置。顾名思义，该设置会在守护进程启动的时候发送推送消息(通过sd_notify(3))给systemd。

## Service其他配置节点

-   RemainAfterExit：默认值no

默认值为no，这个设置采用booleean值，可以是0、no、off、1、yes、on等值。它表明服务是否应当被视为激活的，即便当它所有的进程都退出了。简言之，这个设置用于告诉systemd服务是否应当是被视为激活状态，而不管进程是否退出。当为true时，即便服务退出，systemd依然将这个服务视为激活状态，反之则服务停止。

-   GuessMainPID

采用boolean值指定systemd在无法确切的查明服务的时候是否需要猜测服务的main pid。除非Type=forking被采用并且PIDFile没有被设置，否则这个选项会被忽略。因为当设置为Type的其他选项，或者显示的指定了PID文件后，systemd总是能够知道main pid。

-   PIDFile

采用一个绝对路径的文件名指定守护进程的PID文件。当Type=forking被设置的时候，建议采取这个设置。当服务启动后，systemd会读取守护进程的主进程id。systemd不会对该文件写入数据。

-   BusName

使用一个D-Bus的总线名称,作为该服务的可访问名称。当Type=dbus的时候，该设置被强制使用。

-   BusPolicy

如果该选项被指定，一个自定义的kdbus终结点将会被创建，并且会被指定为默认的dbus节点安装到服务上。这样的自定义终结点自身持有一个策略规则集合。这些规则将会在总线范围内被强制指定。该选项只有在kdbus被激活时有效。

-   ExecStart

当服务启动的时候（systemctl start youservice.service），会执行这个选项的值，这个值一般是“ExecStart=指令 参数”的形式。当Type=oneshot的时候，只有一个指令可以并且必须给出。原因是oneshot只会被执行一次。

-    ExecStartPre、ExecStartPost 

顾名思义，这两个设置的意义在于ExecStart被执行之前和之后被执行。

-   ExecReload

服务重启时执行。

-   ExecStop

服务停止时执行。

-   ExecStopPost

服务停止后执行。

## VNCServer 服务写法

*不可用*

```shell
[Unit]
Description=VNCServer - TightVNCServer
Documentation=man:vncserver(8)
DefaultDependencies=no
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/bin/vncserver -kill :1 >/dev/null && /usr/bin/vncserver >/dev/null
ExecStop=killall Xtightvnc

[Install]
WantedBy=multi-user.target
```

