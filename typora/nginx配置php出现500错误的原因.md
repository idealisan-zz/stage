# nginx配置php出现500错误的原因

nginx配置php只需要将下面的内容添加到nginx.conf中的server段即可，但是有时候会出现500错误，无法使用phpinfo.php测试配置正确性。

```nginx
        #除下面提及的需要添加的配置信息外，其他配置保持默认值即可。
        location / {
            #在location大括号内添加以下信息，配置网站被访问时的默认首页
            index index.php index.html index.htm;
        }
        #添加下列信息，配置Nginx通过fastcgi方式处理您的PHP请求
        location ~ .php$ {
            root /usr/share/nginx/html;    #将/usr/share/nginx/html替换为您的网站根目录
            fastcgi_pass 127.0.0.1:9000;   #Nginx通过本机的9000端口将PHP请求转发给PHP-FPM进行处理
            fastcgi_index index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include fastcgi_params;   #Nginx调用fastcgi接口处理PHP请求
        }                
```

出现错误的原因其实是上边这段代码放置的位置不对，正确的配置中要放在`include /etc/nginx/default.d/*.conf`之后。

![nginx-configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2490664751/p69697.png)

要先使用包含进来的配置文件的配置匹配并处理http请求然后将处理过的请求发送给php-fpm进一步处理，如果不匹配再尝试直接使用php-fpm处理。

错误的配置中上来就用php-fpm处理则会出现错误。