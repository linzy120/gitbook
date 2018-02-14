# Apache设置

`在根目录下新建 .htaccess 文件`

## 基本设置
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

## 防盗链设置
```
RewriteEngine On
#允许所有空来源的都能访问
#RewriteCond %{HTTP_REFERER} !^$ [NC]
#允许local.chuanxiao来源的访问
RewriteCond %{HTTP_REFERER} !local.chuanxiao [NC]
#允许google来源的访问
#RewriteCond %{HTTP_REFERER} !google.com [NC]
#允许baidu来源的访问
#RewriteCond %{HTTP_REFERER} !baidu.com.com [NC]
#禁止访问(访问以下格式的文件)的进行重写向
RewriteRule .*.(gif|jpg|png)$ http://local.chuanxiao[R,NC,L]
```


## 完整设置
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]

RewriteEngine On
RewriteCond %{HTTP_REFERER} !local.chuanxiao [NC]
RewriteRule .*.(gif|jpg|png)$ http://local.chuanxiao[R,NC,L]
```
