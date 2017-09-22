---
title: hexo+github搭建免费网站
date: 2016-12-10 19:20:33
updated: 2016-12-10 19:20:33
tags: [hexo,环境搭建]
categories: 环境搭建
---

### 一、准备工作
 * 一个github账号
 * 安装node.js(node官网下载)、npm(推荐brew安装)
 * 安装git

### 二、在GitHub创建仓库
 * 新建一个名为你的用户名.github.io的仓库
 * 仓库名字必须是：username.github.io，其中username是你的用户名

### 三、安装hexo
 * Hexo是一个简单、快速、强大的基于 Github Pages 的博客发布工具，支持Markdown格式，有众多优秀插件和主题。
 * 官网：http://hexo.io
 * github: https://github.com/hexojs/hexo
 * 安装 npm install -g hexo
 * 初始化 在电脑的某个地方新建一个名为blog的文件夹（名字可以随便取），我的是~/Sites/blog,进入目录执行 hexo init
 至此hexo安装完毕
 
### 四、本地运行
 * 在项目根目录下执行 hexo s #启动服务
 * 访问：http://localhost:4000

### 五、配置git
 * 编辑 根目录下的 _config.yml 文件
 {% codeblock %}
 deploy:
     type: git
     repo: https://github.com/username/username.github.io  #username就是你的用户名
     branch: master
     message: 提交博客内容
 {% endcodeblock %}
 * 安装插件 npm install hexo-deployer-git --save
 * 提交 hexo d  如果提交失败需要清除 hexo clean && hexo g && hexo d

### 六、下载好看的主题和有用的插件
 * Plugins: https://hexo.io/plugins/
 * Themes: https://hexo.io/themes/
 * 主题里面自带使用教程这里就不多说了

### 七、绑定域名(依据个人情况)
 * 如果不绑定域名肯定也是可以的，就用默认的 xxx.github.io 来访问
 * 阿里云购买域名后 执行ping xxx.github.com得到对应的IP地址，去万网的控制台中解析购买的域名
 * 在blog/source 目录下新建一个CNAME文件(无后缀名) 将自己的域名 www.lxzfranky.com填入其中,至此自己的博客基本已经完成

### 八、其他一些设置
 * 详见根目录下的 _config.yml,另外会单独写一篇文章来讲解如何设置
 
### 九、开始你的博客之旅吧
 * 到根目录下执行如下命令hexo会帮我们在_posts下生成相关md文件，打开这个文件编辑就好了
    {% codeblock %}
    执行hexo new 'first'命令
    打开文件如下：
    ---
    title: postName #文章页面上的显示名称，一般是中文
    date: 2017-07-07 19:39:39 #文章生成时间，一般不改，当然也可以任意修改
    categories: 默认分类 #分类
    tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
    description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
    ---
    以下是正文
    {% endcodeblock %}
    
### 十、hexo一些常用命令
 * 基本命令
    1. hexo new "postName" #新建文章
    2. hexo new page "pageName" #新建页面
    3. hexo generate #生成静态页面至public目录
    4. hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
    5. hexo deploy #部署到GitHub
    6. hexo help  # 查看帮助
    7. hexo version  #查看Hexo的版本
 * 缩写
    1. hexo n == hexo new
    2. hexo g == hexo generate
    3. hexo s == hexo server
    4. hexo d == hexo deploy
 * 命令组合
    1. hexo s -g #生成并本地预览
    2. hexo d -g #生成并上传

