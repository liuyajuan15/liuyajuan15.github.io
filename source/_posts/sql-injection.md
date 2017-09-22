---
title: SQL注入
date: 2017-09-07 22:01:39
tags: [安全，SQL注入]
categories: 安全
---
### 一、目的简介
 * 让这些输入被认为是一个SQL查询，或者是查询的一部分,针对程序员编程的忽略通过SQL语句，实现无账号登录甚至篡改数据库。

### 二、SQL注入攻击的总体思路：
1. 寻找到SQL注入的位置
2. 判断服务器类型和后台数据库类型
3. 针对不同的服务器和数据库特点进行SQL注入攻击

### 三、SQL注入攻击实例
```php
    免账号登录：
    sql = "select * from user_table where username=' "+userName+" ' and password=' "+password+" '";
    当用户输入的用户名为 ‘or 1=1 --
    sql = "select * from user_table where username=‘’ or 1=1 -- ' and password=' "+password+" ‘";
    由于1=1肯定会成功 ，--是注释后面的语句不用执行，因此不用账号密码就可以登录成功。
    当用户输入的用户名为 ' ;DROP DATABASE (DB Name) —'
    以上语句则会删除数据库
```

### 四、防范：
* 服务器配置防范
    1. 关闭注册全局变量，在PHP中提交的变量，包括post或者get提交的变量，都将自动注册为全局变量，能够直接访问，对服务器是不安全的，所以应该关闭 register_globals=off
    2. 打开magic_quotes_gpc 防止SQL注入。 默认是关闭的，如果打开后将自动把用户提交对SQL的查询进行转换，比如把’ 转为\’ 等
    3. 错误信息控制 一般数据库会在没有连接到数据库或者在其他情况下会有提示错误，一般错误信息中会包含PHP脚本当前的路径信息或者查询的SQL语句信息，因此display_errors = off ，关闭错误信息后把错误日志打开
* PHP方法：
    1. addslashes() 强行加\
    2. mysql_real_escape_string()会判断字符集，但是对PHP版本有要求
    3. mysql_escape_string() 不考虑连接的当前字符集