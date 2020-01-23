# Nginx中搭建WordPess出现下载而不是访问php的解决办法

## 确认开启了php的fastcgi配置

在Nginx的配置文件中，在nginx.conf或者站点的conf或者任何一个用于定位到WordPress的配置文件中，开启下边的配置，可能是取消注释就好了，也可能要自己添加：

```nginx
	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
	
		# With php-fpm (or other unix sockets):
		fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
		# With php-cgi (or other tcp sockets):
		#fastcgi_pass 127.0.0.1:9000;
	}

```

## 记得WordPress根目录添加index index.php

如果使用的是一个域名的根目录配置了`root /.../wordpress`或者没有wordpress文件夹而是直接把里边的文件放在了目录里，那么只要在server中的index后边加上index.php就好了。

如果使用的是子目录放置wordpress，可能在访问wordpress目录的时候，例如http://localhost/wordpress，可能浏览器会下载一个文件，该文件内容是php文件内容，而不是渲染一个网页。这种情况下要在wp-config.php所在的位置加上额外的`index index.php`。

例如，我将wordpress的文件，例如wp-config.php位于wordpress/wp-config.php，则需要在server 下有如作为的location配置：

```nginx
	location /wordpress{
		index index.php index.html index.htm;
	}
```

