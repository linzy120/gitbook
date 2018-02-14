# Nginx设置


### LNMP相关配置文件位置

> Nginx主配置(默认虚拟主机)文件：`/usr/local/nginx/conf/nginx.conf`  
> 添加的虚拟主机配置文件：`/usr/local/nginx/conf/vhost/域名.conf`  
> PHP配置文件：`/usr/local/php/etc/php.ini`  
> php-fpm配置文件：`/usr/local/php/etc/php-fpm.conf`  

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
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_param   PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        include fastcgi.conf;
    }

    location /nginx_status {
        stub_status on;
        access_log  off;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
        expires      30d;
        valid_referers server_names;
        if ($invalid_referer) {
            return 403;
        }
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

### 配置说明

1. `try_files $uri $uri/ /index.php?$query_string`  
注意后面的`?$query_string`, 意思为重定向仍保持url参数, 如果删除则抛弃参数  

2. PHP的设置在`location ~ [^/]\.php(/|$)`内, 还不是特别明白, 且这么弄吧 

3. 图片等其他文件的防盗链设置  
```
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$ {
    expires      30d;
    valid_referers server_names;
    if ($invalid_referer) {
        return 403;
    }
}
```
其他格式的图片或文件的类型可自行添加到location后的括号内  
valid_referers表示白名单, 有以下值可以设置  
+ none  
`Referer可为空`
+ blocked       
`Referer不可为空，但是里面的值被代理或者防火墙删除了，这些值都不以http://或者https://开头，而是“Referer: XXXXXXX”这种形式`
+ server_names  
`Referer来源头部包含当前的server_names（当前域名）`
+ 任意字符串  
`定义服务器名或者可选的URI前缀.主机名可以使用*开头或者结尾，在检测来源头部这个过程中，来源域名中的主机端口将会被忽略掉。如: *.domain.com ~\.google\. ~\.baidu\.`
+ 正则表达式  
`~表示排除https://或http://开头的字符串.`  


`以上值可以混用, 用空格隔开`  
注: `Referer`: `HTTP Referer`是`header`的一部分, 告诉服务器我是从哪个页面链接过来的。但通过Referer实现防盗链比较基础，仅可以简单实现方式资源被盗用。构造`Referer`的请求很容易实现。  
`$invalid_referer` 是否非法: 0合法, 1非法。在valid_referers白名单内的为合法,反之则非法 
如果非法的话, 可以直接返回403, 也可以返回指定图片, 代码如下:  
```
if ($invalid_referer) {
    rewrite ^/ http://local.chuanxiao/图片名.jpg;
    #return 403;
}
```

4. 禁止访问指定目录  
`如要禁止访问根目录下的resources目录, 可以这么写`
```
location /resources/ {
    deny all;
}
```
