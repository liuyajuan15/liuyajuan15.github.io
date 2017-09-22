---
title: PHP字符串
date: 2017-09-20 16:28:52
categories: PHP
tags: [PHP,字符串]

---

>一个字符串 string 就是由一系列的字符组成，其中每个字符等同于一个字节。这意味着 PHP 只能支持 256 的字符集，因此不支持 Unicode 

一个字符串可以用以下四种方式表达：

* 单引号
* 双引号
* Heredoc 语法结构
* Nowdoc 语法结构（自 PHP 5.3.0 起）

### 单引号
>定义一个字符串的最简单的方法是用单引号把它包围起来('),当表达一个单引号自身，需要用反斜线(\)来转义，表达一个反斜线，需要两个反斜线。其他任何方式出现的反斜线都被会当成反斜线本身。**单引号中的变量和除了转义(',\)的其他特殊字符都不会被转义**

```php
echo 'this is a simple string';

// 可以录入多行
echo 'You can also have embedded newlines in 
strings this way as it is
okay to do';
echo 'Arnold once said: "I\'ll be back"'; // 输出： Arnold once said: "I'll be back"   \' 输出'
echo 'You deleted C:\\*.*?'; // 输出： You deleted C:\*.*?
echo 'You deleted C:\*.*?'; // 输出： You deleted C:\*.*?
echo 'This will not expand: \n a newline'; // 输出： This will not expand: \n a newline  \n不会被转义
echo 'Variables do not $expand $either'; // 输出： Variables do not $expand $either   变量不会被解析
```

### 双引号

>定义一个字符串还可以包围在双引号(")中，双引号中PHP会对一些特殊的字符进行解析  \n,\r,\t,\v,\e,\f,\\,\$,\",\[0-7]{1,3},\x[0-9A-Fa-f]{1,2}  **双引号可以解析变量，除了前面的特殊字符其他字符的转义反斜线都会被显示出来**

```php
<?php
echo "this is test";   //this is test
echo "this is 
test
test ";  //this is test test 
echo "Arnold once said: \"I'll be back\""; //Arnold once said: "I'll be back"
echo "You deleted C:\\*.*?";  //You deleted C:\*.*?
echo "You deleted C:\*.*?";  You deleted C:\*.*?
echo "This will not expand: \n a newline";  //This will not expand: a newline
$expand="one";
$either="two";
echo "Variables do not $expand $either";  //Variables do not one two
```

### heredoc 
><<<EOT结构，注意结束标识符这行除了可能有一个分号外，绝对不能包含其他字符，不能缩进，不能有任何空白或制表符 **Heredoc 就像是没有使用双引号的双引号字符串，变量可以解析，转义序列也可以使用，也可以用来初始化静态变量和类的属性和常量**

```php
$str = <<<EOD
Example of string
spanning multiple lines
using heredoc syntax.
EOD;
//
```

### Nowdoc
>类似于Heredoc,但是开始必须是<<<'EOT',标识符要用单引号引起来**变量并不会被解析**

```PHP
$str = <<<'EOD'
Example of string
spanning multiple lines
using nowdoc syntax.
EOD;
```

### 字符串转换为数值

>当一个字符串被当做一个数值来取值。如果该字符串没有包含'.','e','E' 并且其数字值在整型的范围之内，该字符串被当做整型来取值，其他所有情况下都被作为浮点数来取值。字符串的开始部分决定了它的值。

```php
<?php
$foo = 1 + "10.5";                // $foo is float (11.5)
$foo = 1 + "-1.3e3";              // $foo is float (-1299)
$foo = 1 + "bob-1.3e3";           // $foo is integer (1)
$foo = 1 + "bob3";                // $foo is integer (1)
$foo = 1 + "10 Small Pigs";       // $foo is integer (11)
$foo = 4 + "10.2 Little Piggies"; // $foo is float (14.2)
$foo = "10.0 pigs " + 1;          // $foo is float (11)
$foo = "10.0 pigs " + 1.0;        // $foo is float (11) 
```

### 转换成字符串

* 布尔型的TRUE被转换成‘1’，FALSE被转换成‘0’
* 整型，浮点型 被转换成数字的字面样式的string
* 数组总是转换成字符串'Array'
* 对象总是被转换成字符串 "Object"
* 资源 resource 总会被转变成 "Resource id #1" 这种结构的字符串
* NULL总是被转变成空字符串
* 大部分的 PHP 值可以转变成 string 来永久保存，这被称作串行化，可以用函数 serialize() ，json_encode ()， var_export($items, true);都可以实现

### 字符串编码
**ASCII编码**
>我们知道，一个二进制位(Bit)有0、1两种状态，一个字节(Byte)有8个二进制位，有256种状态，每种状态对应一个符号，就是256个符号，从0000000到11111111。计算机诞生于美国，早期的计算机使用者大多使用英文，上世纪60年代，美国制定了一套英文字符与二进制位的对应关系，称为**ASCII码**，沿用至今

**GB2312和GBK编码**
>等中国人们得到计算机时，已经没有可以利用的字节状态来表示汉字，况且有6000多个常用汉字需要保存呢。但是这难不倒智慧的中国人民，我们不客气 地把那些127号之后的奇异符号们直接取消掉, 规定：一个小于127的字符的意义与原来相同，但两个大于127的字符连在一起时，就表示一个汉字，前面的一个字节（他称之为高字节）从0xA1用到 0xF7，后面一个字节（低字节）从0xA1到0xFE，这样我们就可以组合出大约7000多个简体汉字了。在这些编码里，我们还把数学符号、罗马希腊的 字母、日文的假名们都编进去了，连在 ASCII 里本来就有的数字、标点、字母都统统重新编了两个字节长的编码，这就是常说的”全角”字符，而原来在127号以下的那些就叫”半角”字符了。 中国人民看到这样很不错，于是就把这种汉字方案叫做 **“GB2312“**。GB2312 是对 ASCII 的中文扩展。

>但是中国的汉字太多了，我们很快就就发现有许多人的人名没有办法在这里打出来，特别是某些很会麻烦别人的国家领导人。于是我们不得不继续把 GB2312 没有用到的码位找出来老实不客气地用上。 后来还是不够用，于是干脆不再要求低字节一定是127号之后的内码，只要第一个字节是大于127就固定表示这是一个汉字的开始，不管后面跟的是不是扩展字 符集里的内容。结果扩展之后的编码方案被称为 **GBK** 标准，GBK包括了GB2312 的所有内容，同时又增加了近20000个新的汉字（包括繁体字）和符号


**Unicode（万国码，国际码，统一码）**
>随着计算机的流行，使用计算机的人越来越多，不仅限于美国，整个世界都在使用,因为当时各个国家都像中国这样搞出一套自己的编码标准，结果互相之间谁也不懂谁的编码，谁也不支持别人的编码。正在这时，大天使加百列及时出现了——一个叫 ISO （国际标谁化组织）的国际组织决定着手解决这个问题。他们采用的方法很简单：废了所有的地区性编码方案，重新搞一个包括了地球上所有文化、所有字母和符号 的编码！他们打算叫它”Universal Multiple-Octet Coded Character Set”，简称 UCS, 俗称 “unicode“。这就是
**Unicode编码（Unique Code），也称统一码、万国码**。

**UTF-8 和 Unicode的关系**
>* Unicode是一种字符编码方法* 实际传输过程中, 不同系统平台的设计不一定一致* 不同的编码空间利用率不同* UTF-8是为了传输而设计* UTF-8是一种针对Unicode的可变长度字符编码* UTF-8是以8位为基本编码单位
* 注意的是unicode一个中文字符占2个字节，而UTF-8一个中 文字符占3个字节

**GB2312和GBK编码**
>* GB2312由中国国家标准总局发布* 古汉语的罕用字和繁体字，GB2312不能处理* 收录的汉字已经覆盖中国大陆99.75%的使用频率。 * 注意GB2312/GBK是定长编码（ascii字符占1个字节）* GBK不是标准，它对罕见字，繁体字都支持

**UTF8，GBK之间的转换与选择**
>* GBK、GB2312等与UTF8之间都必须通过Unicode编码才能相互转换
>* 优先选择UTF-8，对程序员有利
>* 为了节省存储空间，选择GBK>* 考虑兼容其它系统，选择GBK>* 要给外国人看(韩国人)，就选 UTF-8>* 需要支持多语言，得选择UTF-8

### 字符串函数
**多字节转码函数 mbstring 和 iconv**

* 优先选择mbstring * 为了追求速度，可以用iconv* mbstring功能更强大，考虑的编码类型更多* mbstring有55个函数，iconv只有11个

**输入输出函数**

* echo —输出一个或多个字符串* print —输出字符串* vprintf —输出格式化字符串* printf —将格式化后的字符串写入到流* sprintf —返回格式化字符串* vsprintf —同sprintf，但是接收一个数组参数* sscanf —根据指定格式解析输入的字符

**查找字符位置函数**

* strpos — 查找字符串首次出现的位置* stripos —不区分大小写* strrpos — 查找最后一次出现的位置* strripos —不区分大小写

**字符串替换函数**

* str_replace — 子字符串替换* substr_replace — 替换字符串的子串

**字符串分割和合并相关**

* explode — 使用一个字符串分割另一个字符串* implode — 将一个一维数组的值转化为字符串* join — 别名 implode* str_getcsv — 解析 CSV 字符串为一个数组* str_split — 将字符串转换为数组* strtok — 标记分割字符串* chunk_split — 将字符串分割成小块* parse_str — 将字符串解析成多个变量

**字符串去除空白字符**

 * trim — 去除字符串首尾处的空白字符 * ltrim — 删除字符串开头的空白字符 * rtrim — 删除字符串末端的空白字符

**HTML代码相关**
 
* nl2br — 换行转为<br />* htmlentities —转化所有可能的html字符(编码)* html_entity_decode* htmlspecialchars — 转化html几个特有的字符* htmlspecialchars_decode

**数据库相关**

* addslashes — 使用反斜线引用字符串* stripslashes — 反引用* addcslashes — 转义制定字符串中的字符* stripcslashes — 反引用* mysql_real_escape_string

**URL字符串函数**

* base64_decode — 对使用 base64 编码的数据进行解码* base64_encode — 使用 MIME base64 对数据进行编码* http_build_query — 生成 URL-encode 之后的请求字符串* parse_url — 解析 URL，返回其组成部分* rawurldecode — 对已编码的 URL 字符串进行解码* rawurlencode — 按照 RFC 1738 对 URL 进行编码* urldecode — 解码已编码的 URL 字符串* urlencode — 编码 URL 字符串

**字符类型函数**
* ctype_alpha    是否字母 ，全部是才返回true，否则返回false* ctype_lower    是否小写 ，全部是才返回true，否则返回false* ctype_upper   是否大写 ，全部是才返回true，否则返回false* ctype_digit     是否数字 ，全部是才返回true，否则返回false

### 字符串安全处理
```php
//过滤危险的HTML(默认级别)
$str = preg_replace("#script:#i", "s c r i p t :", $str);
$str = preg_replace("#[\/]{0,1}(link|meta|ifr|fra|src)[^>]*#isU", '', $str);
$str = preg_replace("#[\r\n\t]{1,}#", '', $str);

//完全禁止HTML并转换一些不安全字符串
$str = addslashes(htmlspecialchars(stripcslashes($str)));
$str = preg_replace("#eval#i", 'e v a l', $str);
$str = preg_replace("#union#i", 'u n i o n', $str);
$str = preg_replace("#concat#i", 'c o n c a t', $str);
$str = preg_replace("#--#", '- - ', $str);
$str = preg_replace("#[\r\n\r]{1,}#", '', $str);
```