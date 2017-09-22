---
title: 深拷贝和浅拷贝
date: 2017-09-08 18:12:41
tags: 设计模式
categories: 设计模式

---
* 深拷贝：赋值时完全复制，完全的copy，对其中一个做出改变，不会影响另一个
* 浅拷贝：赋值时，引用赋值，相当于取了一个别名。对其中一个修改，会影响另一个。
* PHP中， = 赋值时，普通对象是深拷贝，但对对象来说，是浅拷贝。也就是说，对象的赋值是引用赋值。（对象作为参数传递时，也是引用传递，无论函数定义时参数前面是否有&符号）
* php4中，对象的 = 赋值是实现一份副本，这样存在很多问题，在不知不觉中我们可能会拷贝很多份副本。
* php5中，对象的 = 赋值和传递都是引用。要想实现拷贝副本，php提供了clone函数实现。
* clone完全copy了一份副本。但是clone时，我们可能不希望copy源对象的所有内容，那我们可以利用__clone来操作。
* 在__clone（）中，我们可以进行一些操作。注意，这些操作，也就是__clone函数是作用于拷贝的副本对象上的

```php
实现对象这种的深拷贝，有两种方法：
例子1：重写clone函数
<?php
class Test{
    public $a=1;
}
 
class TestOne{
    public $b=1;
    public $obj;
    //包含了一个对象属性，clone时，它会是浅拷贝
    public function __construct(){
        $this->obj = new Test();
    }
     
    //方法一：重写clone函数
    public function __clone(){
        $this->obj = clone $this->obj;
    }
}
 
$m = new TestOne();
$n = clone $m;
 
$n->b = 2;
echo $m->b;//输出原来的1
echo PHP_EOL;
//可以看到，普通属性实现了深拷贝，改变普通属性b，不会对源对象有影响
 
//由于改写了clone函数，现在对象属性也实现了真正的深拷贝，对新对象的改变，不会影响源对象
$n->obj->a = 3;
echo $m->obj->a;//输出1，不随新对象改变，还是保持了原来的属性
 
例子2：序列化和反序列化

class Test{
    public $a=1;
}
 
class TestOne{
    public $b=1;
    public $obj;
    //包含了一个对象属性，clone时，它会是浅拷贝
    public function __construct(){
        $this->obj = new Test();
    }
     
}
 
$m = new TestOne();
//方法二，序列化反序列化实现对象深拷贝
$n = serialize($m);
$n = unserialize($n);
 
$n->b = 2;
echo $m->b;//输出原来的1
echo PHP_EOL;
//可以看到，普通属性实现了深拷贝，改变普通属性b，不会对源对象有影响
 
 
$n->obj->a = 3;
echo $m->obj->a;//输出1，不随新对象改变，还是保持了原来的属性,可以看到，序列化和反序列化可以实现对象的深拷贝
  
例子3：json_encode之后再json_decode,实现赋值
```