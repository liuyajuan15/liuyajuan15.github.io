---
title: hexo个人博客多电脑编辑
date: 2017-09-22 21:56:17
tags: [hexo,环境搭建]
categories: 环境搭建
---
>由于每天上下班带电脑太过麻烦，因此在家里放了一台电脑，那么问题来了，怎么才能在多台电脑上来更新博客，经过百度尝试成功了， 因此记录下来。

### 环境搭建
1. 在原有的仓库https://github.com/liuyajuan15/liuyajuan15.github.io 下面在创建一个hexo 分支，并设置为默认分支，之前默认为master
2. 在本地执行└─>[娟姐 😜 👉 git:(hexo*) ]🤑  好好挣钱 🤑️ >> git clone https://github.com/liuyajuan15/liuyajuan15.github.io.git
3. 进入liuyajuan15.github.io目录，把原来目录的文件拷贝过来，执行git add --all,git commit -m 'change' ,git push origin hexo
4. 查看一下_config.yml中的deploy参数分支是否是master，默认下是master
5. 执行hexo clean && hexo g && hexo d 把生成的文件提交到master分支上,这样hexo用来存放网站的原始文件，master分支用来存放生成的静态页面。

### 日常改动
>平时写博客只需要hexo clean && hexo g && hexo d 执行这个命令，把静态文件部署即可，现在需要多做一步操作

1. git branch 查看当前分支是否为hexo，不做修改的情况下默认为hexo
2. 依次执行git add .、git commit -m "..."、git push origin hexo 将改动推送到github
3. 执行hexo clean && hexo g && hexo d

### 新电脑使用流程
1. 在工作目录下执行git clone https://github.com/liuyajuan15/liuyajuan15.github.io.git
2. 在本地安装node.js,npm,git等
3. 依次执行npm install hexo,   npm install,   npm install hexo-deployer-git,不需要hexo init这条指令
4. 遇到了一些问题，解决办法请参考http://www.midaoi.com/2016/10/27/hexoError/