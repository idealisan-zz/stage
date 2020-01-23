Centos使用firewalld，比较复杂，不能用ufw真是很麻烦。

centos开启防火墙某端口使用如下命令，将端口对互联网开放：

```shell
sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --reload
```

