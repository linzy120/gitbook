# 编码风格

- [为什么](#为什么)
- [常用的几种命名规则](#常用的几种命名规则)
- [文件名命名](#文件名命名)
- [类名命名](#类名命名)
- [方法名命名](#方法名命名)
- [变量命名](#变量命名)
- [注释](#注释)
    - [文件注释](#文件注释)
    - [函数注释](#函数注释)
    - [变量注释](#变量注释)




## 为什么
所谓代码风格统一，既浪费时间也没有实际意义。
那我他娘的为什么要这样做呢?
- [ ] 为了看起来舒服?
- [ ] 为了提升装逼档次?
- [ ] 为了方便他人维护(关我屁事)?
- [x] 不要问我, 做就对了。


---


## 常用的几种命名规则

+ **大驼峰**  
    `如: LoginController`
+ **小驼峰**  
    `如: name | nickName`
+ **小写下划线**  
    `如: name | nick_name`
+ **大写下划线**  
    `如: Model_users | Model_Users`
+ **首字母大写**  
    `如: Main | Loadview`
+ **全小写**  
    `如: main | loadview`


---


## 文件名命名
| 文件    | 命名方式      | 例如               |
| :----- | :----------- | :----------------- |
| 控制器  | [大驼峰]      | Welcome.php        |
| 类库    | [首字母大写]  | Loadview.php       |
| 模型    | [大写下划线]  | Model_cx_users.php |
| 视图    | [全小写]     | main.php           |

---

## 类名命名
统一使用: [大驼峰]

---

## 方法名命名
统一使用: [小写下划线]

---

## 变量命名
普通变量可自由些(也为了统一早期的变量)  
可使用: [小驼峰]和[小写下划线]  

| 文件     | 命名方式               | 例如                  |
| :------- | :------------------- | :-------------------- |
| 常量     | 全部大写               | BASEPATH              |
| 普通变量  | [小驼峰]和[小写下划线]  | $userName, $user_name |
| 全局变量  | 以g开头的小驼峰        | $gConnectTime         |
| 静态变量  | 以s开头的小驼峰        | $sStatus              |


---


## 注释

### 文件注释
```
/*
 |--------------------------------------------------------------------------
 | 这里为文件标题
 |--------------------------------------------------------------------------
 | 这里为类/文件的细节说明
 | 
 | @Author  公司/个人
 | @Date    日期
 */
```

---

### 函数注释
1. 无参数的简单注释 (若需要复杂备注, 参考有参数的)
```php
//获取菜单项信息
public function get_menu_info() {
    ...
}
```
2. 有参数
```php
/**
 * 获取用户的数据
 * 
 * @param  String       $name
 * @param  String|NUll  $pawd
 * @return Array|NULL   user
 */
public function get_user_data($name, $pawd = null) {
    $this->CI->load->model('Model_cx_users', 'users');
    return $this->CI->users->get_user_info($name, $pawd);
}
```
`注: 文字描述行与参数说明行之间间隔一行, 且第一行为**(2个星, 不然无效)`  
`参数类型的首字母需大写且空2个空格, return空一个空格`

---

### 变量注释
1. 正常在后面添加注释即可
```php
    public $user_name;  //会员姓名
```
2. 若有指定格式的，请说明格式
```php
/***
    $this->except 格式
    $this->except[访问的类名][访问的方法] = array(
            'group' => $this->menuDatas的根键名,
            'child' => $this->menuDatas的根键名下的子键名,
            'infos' => array(
                //可以设置新的菜单信息
                //如: 'name' => '新的标题'
            ),
    );
*/
```
`注: 第一行为三个星号，且中间行的行首没有星号(这样方便复制)`
