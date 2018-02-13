# Nginx设置


### LNMP相关配置文件位置

> Nginx主配置(默认虚拟主机)文件：/usr/local/nginx/conf/nginx.conf  
> 添加的虚拟主机配置文件：/usr/local/nginx/conf/vhost/域名.conf  
> PHP配置文件：/usr/local/php/etc/php.ini  
> php-fpm配置文件：/usr/local/php/etc/php-fpm.conf  

1. php-fpm.conf 添加内容:  
listen = /tmp/php-cgi.sock;  
注: 127.0.0.1:9000 和 /tmp/php-cgi.sock 都是可行的，不过/tmp/php-cgi.sock性能稍微好点

2. 添加配置文件  
注: 为了避免fastcgi_pass重复(重复会报错)  
请把/usr/local/nginx/conf/nginx.conf 原来的server注释掉, 或把server下server_name的默认值 _ 改掉  

3. 重启lnmp  
lnmp restart


### 配置内容
```
server {
    listen 80 default_server;
    #listen [::]:80 default_server ipv6only=on;
    server_name  fengquanxin.com www.fengquanxin.com;
    index index.html index.htm index.php;
    root  /home/wwwroot/chuanxiao/web;

    #error_page   404   /404.html;

    # Deny access to PHP files in specific directory
    #location ~ /(wp-content|uploads|wp-includes|images)/.*\.php$ { deny all; }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ [^/]\.php(/|$) {
        try_files $uri =404;
        fastcgi_pass  unix:/tmp/php-cgi.sock;
        fastcgi_index index.php;
        include fastcgi.conf;
    }

    location /nginx_status {
        stub_status on;
        access_log   off;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
        expires      30d;
    }

    location ~ .*\.(js|css)?$ {
        expires      12h;
    }

    location ~ /.well-known {
        allow all;
    }

    location ~ /\. {
        deny all;
    }

    access_log  /home/wwwlogs/access.log;
}
```


# Apache设置
在根目录下新建 .htaccess 文件, 内容如下:
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```