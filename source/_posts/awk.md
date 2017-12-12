---
title: awk学习
date: 2017-03-08 18:17:23
updated: 2017-03-08 18:17:23
tags: awk
categories: Linux
---
awk是一个强大的文本分析工具，相对于grep的查找，sed的编辑，awk在其对数据分析并生成报告时，显得尤为强大。简单来说awk就是把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分再进行各种分析处理。
使用方法：awk '{pattern + action}' {filenames}

### 一、书写格式：
1. 命令行格式 awk -F":" '{print $2}’ test.txt   使用较多
2. 文本格式 
#!/usr/bin/awk
BEGIN {FS=":"}
{print $1}

以上为写好的文本格式file.awk
执行.awk文件的命令为：awk -f file.awk test.txt

### 二、变量：
常用内置变量

1. $0 全部内容
2. $1~$n 被分割的第n个字符
3. FS 分隔符（默认为空格）
4. RS  输入记录的分隔符(默认为空格)
5. NF 字段个数
6. NR 行号

自定义及外部变量

1. awk -v name=$myname 'BEGIN{print name}’    $myname 是自己定义的变量
2. awk -v host=$HOSTNAME 'BEGIN{print host}’  $HOSTNAME 是系统变量
3. awk -v name=‘lalalal' 'BEGIN{print name}’   还可以直接定义

### 三、操作符
1. ~ 匹配
匹配第7列中是以/bin开头的文件并输出
╰─>awk -F: '$7 ~ /^\/bin/{print $0}' /etc/passwd                                                           17-08-16 11:04
root:*:0:0:System Administrator:/var/root:/bin/sh
_mbsetupuser:*:248:248:Setup User:/var/setup:/bin/bash
2. !~ 不匹配
匹配第7列中不是以/bin开头的文件并输出
╰─>awk -F: '$7 !~ /^\/bin/{print $0}' /etc/passwd                                                           

### 四、输出
1. print:直接打印
2. printf:格式化打印

### 五、流程控制
1. if   seq 10 | awk '{if($0%2==0){print "ok"}else{print"no"}}’
2. while  给etc/passwd 中的每一个字段都加上序号
╭─◆liuyajuan@liuyajuandeMacBook-Pro.local/Users/liuyajuan/Downloads
╰─>awk -F: '{i=1;while(i<=NF){printf(" %d:%s ",i,$i);i++}{print " " }}' /etc/passwd
3. for

### 六、BEGIN ,END
>作用:给程序赋予初始化状态和在程序结束之后执行一些扫尾工作。任何在BEGIN之后列出的操作（在{}内）将在Unix awk开始扫描输入之前执行,而END之后列出的操作将在扫描完全部的输入之后执行因此，通常使用BEGIN来显示变量和预置（初始化）变量，使用END来输出最终结果。

### 七、练习题：
1. 截取IP地址

```php
┌─◆<liuyajuan@liuyajuandeMacBook-Pro> 
└─>[娟姐 😜 👉  ]🤑  好好挣钱 🤑️ >> ifconfig en4
en4: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
	options=4<VLAN_MTU>
	ether 00:e0:4c:36:0b:9c
	inet6 fe80::140f:6090:e77a:14ca%en4 prefixlen 64 secured scopeid 0x4
	inet 172.19.32.5 netmask 0xffffff00 broadcast 172.19.32.255
	nd6 options=201<PERFORMNUD,DAD>
	media: autoselect (100baseTX <full-duplex>)
	status: active
┌─◆<liuyajuan@liuyajuandeMacBook-Pro> 
└─>[娟姐 😜 👉  ]🤑  好好挣钱 🤑️ >> ifconfig en4 | awk -F ' ' '/inet / {print $2}'
172.19.32.5
```

2.统计网络连接数

```php
┌─◆<liuyajuan@liuyajuandeMacBook-Pro> 
└─>[娟姐 😜 👉  ]🤑  好好挣钱 🤑️ >> netstat -an | awk '/^tcp/ {++state[$NF]} END {for(key in state) print key,"\t",state[key]}' | column -t
LISTEN       14
SYN_SENT     2
CLOSED       1
TIME_WAIT    5
ESTABLISHED  12
```






