# 框架申明

## 严重警告

您正在使用CodeIgniter框架  
为了网站安全  
+ 在每一个目录下新建一个`index.html`文件  
```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>403 Forbidden</title>
    </head>
    <body>

    <p>Directory access is forbidden.</p>

    </body>
    </html>
```


+ 在每一个PHP文件的开头添加如下一行代码  

```php
<?php defined('BASEPATH') OR exit('No direct script access allowed'); ?>
```

`不然你网站内的文件可能会直接被URL访问(非正常链接跳转)`