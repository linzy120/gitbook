# PHP设置

```
/*
 |-------------------------------------------------------
 | PHP设置文件的修改记录
 |-------------------------------------------------------
 */
 ```

1. 允许获取XML的POST请求
always_populate_raw_post_data = -1  
注: PHP7已移除, 请用 file_get_contents("php://input") 替代 $GLOBALS['HTTP_RAW_POST_DATA']

2. session自动开启
session.auto_start = 1
注: 如果与其他网站(opencart网站)有冲突的话，可不用开启
但需要设置 session.save_path

3. 修改时间地区
date.timezone = PRC
       
4. 开启短标签
short_open_tag = On

5. 日志设置
error_log = php_error.log
