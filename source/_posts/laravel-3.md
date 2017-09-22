---
title: laravel框架中常用PHP语法
date: 2017-09-08 09:51:06
tags: laravel
categories: 框架分析
---
### 一、文件包含

#### 1.include与require

 * 共同点：include与require关键字用于包含并运行指定文件。
 * 不同点：两者作用几乎一样，只是处理失败的方式不同。require在出错时产生E_COMPILE_ERROR级别的错误，因此会导致脚本程序运行终止，而include则是会产生E_WARNING级别错误，只会发出警告，而脚本程序会继续运行。
 * 查找路径的方式
  {% codeblock %}
   if(如果定义了路径，不管绝对路径还是相对路径){
        include_path会被忽略，按参数给出的路径寻找；
   }else if(只有文件名，没有目录名){
        则按照include_path指定的目录寻找；
   }else if(include_path没有){
        在调用脚本所在的目录和当前工作目录下寻找
   }else{
        Include 发出一条警告，require发生一个致命错误
   }
  {% endcodeblock %}
 * include查找路径的方式比较，推荐由高到低
  1. 通过当前路径查找，好处是迁移到别的项目时什么都不用改
  2. 使用include_path，即默认路径，迁移项目是需要改php.ini
  3. 绝对路径，迁移项目时需要改代码
  4. 相对路径，迁移项目时什么都不用改，但是为什么不推荐，是因为当被多人include时，会存在bug
    1).文件存储如下图所示，当a.php include b.php，c.php include a.php
    2).当单独访问a.php没有问题，但是访问c.php就会报错，找不到b.php

   ![include][id]
    
   [id]: /img/include.jpg "include"
 * 如果包含了一个定义了路径的文件，如include “../file.php”,无论是相对路径还是绝对路径，系统只会在相应的路径下寻找该文件，例如一个文件以“../”开头，则解析器会在当前目录的父目录下寻找该文件。当一个文件被包含时，包含文件则继承了被包含文件拥有的变量，从该处开始，被包含文件可用的任何变量在包含的文件中也都可用，同时在被包含文件中定义的函数，类或者常量都具有全局作用域。
   
#### 2.类的自动加载   
#### 3.laravel中的实现
       
       
  注：以上内容参考《PHP手册》和《laravel框架关键技术解析》以及laravel源代码