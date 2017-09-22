---
title: 单例模式
date: 2017-03-16 21:40:35
updated: 2017-03-16 23:30:07
tags: [设计模式,单例]
categories: 设计模式
img: /img/singleton.jpg
---
一、概览
* 设计原则：无
* 常用场景：应用中有对象需要是全局的且唯一
* 使用概率：99.99999%
* 复杂度：低
* 变化点：无
* 选择关键点：一个对象在应用中出现多个实例是否会引起逻辑上或者是程序上的错误
* 逆鳞：在以为是单例的情况下，却产生了多个实例
* 相关设计模式：
    * 原型模式：单例模式是只有一个实例，原型模式每拷贝一次都会创造一个新的实例。

二、定义：单例模式：单例模式保证一个类只有一个实例，同时这个类还必须提供一个访问该类的全局访问点。

三、UML类图

 ![创建应用][id]
     
 [id]: /img/singleton.jpg "create"
单例模式的三个要点：
1. 需要一个保存类的唯一实例的静态成员变量
2. 构造函数和克隆函数必须私有
3. 必须提供一个访问这个实例的公共静态方法，从而返回唯一实例的一个引用

那么为什么要使用PHP单例模式？

PHP一个主要应用场合就是应用程序与数据库打交道的场景，在一个应用中会存在大量的数据库操作，针对数据库句柄连接数据库的行为，使用单例模式可以避免大量的new操作。因为每一次new操作都会消耗系统和内存的资源。


单例模式代码：
{% codeblock %}
<?php

/**
 * class Singleton
 */
class Singleton
{
    /**
     * @var Singleton reference to singleton instance
     */
    private static $instance;

    /**
     * gets the instance via lazy initialization (created on first usage)
     *
     * @return self
     */
    public static function getInstance()
    {

        if (null === static::$instance) {
            static::$instance = new static;                          //static 相当于self
        }

        return static::$instance;
    }

    /**
     * is not allowed to call from outside: private!
     *
     */
    private function __construct()
    {

    }

    /**
     * prevent the instance from being cloned
     *
     * @return void
     */
    private function __clone()
    {

    }

    /**
     * prevent from being unserialized
     *
     * @return void
     */
    private function __wakeup()
    {

    }
}
{% endcodeblock %}


单例模式优缺点：
优点：
1. 由于在内存中只有一个实例，减少了内存开支，特别是一个对象需要频繁的创建，销毁时
2. 减少了系统的性能开销，读取配置，产生其他依赖对象时，可以通过在应用启动时直接产生一个单例对象。
3. 可以避免对资源的多重占用，比如写文件动作，由于只有一个实例在内存中，可以避免对同一个资源文件的同时写操作
4. 可以在系统设置全局的访问点，优化和共享资源访问，可以设计一个单例类，负责所有的数据表的映射处理
缺点：
1.一般没有接口，扩展困难，
