松茸允许使用端口复用代理隐藏效果，只需要配置好web服务，甚至转发http请求即可。

为了开启这一功能，只需要修改松茸的配置文件即可。

使用的shadowsock-all.sh安装的松茸，其配置文件位于：

`/etc/shadowsocks-r/config.json`

在配置好一个网站之后，如下修改该json文件即可。

```json
{
    "server":"0.0.0.0",
    "server_ipv6":"::",
    "port_password":{
        "12345":"mypassword",
	    "12346":"mypassword2"
    },
    "local_address":"127.0.0.1",
    "local_port":1080,
    "timeout":120,
    "method":"chacha20",
    "protocol":"auth_aes128_md5",
    "protocol_param":"",
    "obfs":"http_simple",
    "obfs_param":"",
    "redirect":"",
    "dns_ipv6":true,
    "fast_open":true,
    "workers":1,
    "redirect":["*:12345#www.smaple.com:80","*:12346#example.com:80"]
}
```

其中最后的json数组中的每个项目形如：

`ssrhost:ssrport#httphost:httpport`

其中的ssrhost可以用星号做通配符，不判断host的名称。

如果可以使用IPv6，可以开启dns_ipv6,否则令其为false。