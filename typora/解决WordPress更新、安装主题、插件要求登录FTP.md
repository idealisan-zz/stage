# 解决WordPress更新、安装主题、插件要求登录FTP

在wordpress/wp-config.php文件的最后一行加上：

```php
define('FS_METHOD', "direct");
```

