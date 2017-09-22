---
title: hexo相关配置以及遇到的问题
date: 2016-12-11 19:20:33
updated: 2016-12-11 19:20:33
tags: [hexo,环境搭建]
categories: 环境搭建
---

### 一、如何统计阅读量
   对着配置看了半天都不知道该去如何设置文章的点击数,看如下的配置阅读量统计也是开启的，怎么不能页面不能显示呢，后来百度才发现是自己太天真了，原来leancloud是需要去注册才能使用的😂😂😂
   {% codeblock %}
    ## leancloud --- leancloud 阅读量统计
    ## {@leancloud:{enable:是否开启,className:创建的class,app_id:,app_key:,region:默认为中国地区,limits:热门文章显示总数}}
    leancloud:
      enable: true
      className: "baseCounter"
      app_id: ''
      app_key: ''
      region:
      limits: 10
   {% endcodeblock %}    
   #### 注册<a href="https://leancloud.cn/">LeanCloud</a>，这里不再赘述
   #### 创建应用
   ![创建应用][id]
     
   [id]: /img/create.png "create"
   
   #### 创建阅读统计表
   ![创建class][id2]
        
   [id2]: /img/class.png "class"
   
   ![counter][id3]
           
   [id3]: /img/counter.png "counter"
   
   #### 获取AppID和AppKey
   ![获取key][id4]
  
   [id4]: /img/appkey.png "appkey"
   
   #### 更改配置
   {% codeblock %}
    ## leancloud --- leancloud 阅读量统计
    ## {@leancloud:{enable:是否开启,className:创建的class,app_id:,app_key:,region:默认为中国地区,limits:热门文章显示总数}}
    leancloud:
      enable: true
      className: "创建的名字"
      app_id: '刚刚拷贝的app_id'
      app_key: '刚刚拷贝的app_key'
      region:
      limits: 10
   {% endcodeblock %} 
   
### 二、如何在文章中插入图片
       其实这里就是markdown的语法了，无奈我不会，因此边学边用图片的语法和链接很像。
   {% codeblock%} 
    行内形式（title 是选择性的）：
    
    ![alt text](/path/to/img.jpg "Title")
    
    参考形式：
    
    ![alt text][id]
    
    [id]: /path/to/img.jpg "Title"
    
    上面两种方法都会输出 HTML 为：
    
    <img src="/path/to/img.jpg" alt="alt text" title="Title" />
   {% endcodeblock%}