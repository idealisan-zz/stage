# WordPress备份迁移和子域名转子目录

因为要把博客从VPS上下载并转移到本地的机器上，而本地的机器没有可以使用的公网IP，更没有DNS提供商能够把我的域名解析到我的电脑上，所以无法使用域名了，只能使用子目录作为web服务的区分方式。

## 备份MySQL数据库

使用如下命令备份数据库到SQL文件：

```shell
mysqldump -uroot -p database_name > output_backup.sql
```

备份得到的SQL文件中不包含创建数据库的命令，只有向数据库添加表的命令，所以还要编辑SQL文件，向其中最开头添加创建数据表的命令，同时附带其他的数据库属性，比如编码等。也为后续的命令指定要使用的数据库。

```sql
CREATE DATABASE IF NOT EXIST 'database_name' DEFAULT CHARACTER SET uft8 COLLATE utf8_general_cli;
USE database_name;
```



### 参考链接：

1.    [WordPress Docker化迁移实战](https://www.yanning.wang/archives/637.html)
2.   [WordPress跨平台迁移实战](https://www.yanning.wang/archives/192.html)
