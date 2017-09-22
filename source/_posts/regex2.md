---
title: 正则表达式测试题
date: 2017-09-13 10:28:41
tags: 正则表达式
categories: 正则表达式

---
> 正则掌握程度测试题,以下测试题来自于 https://www.zybuluo.com/Zjmainstay/note/709093， 答案是我自己的分析，欢迎各位进行批评与指导,部分习题答案后续不上

* [一.分组提取/非捕获组](#one)
* [二.单字符或](#two)
* [三.多字符或](#three)
* [四.分组引用](#four)
* [五.匹配换行数据](#five)
* [六.存在（或）](#six)
* [七. 存在（与）](#seven)
* [八. 特殊限制（环视否定）](#eight)
* [九. 替换分组使用](#nine)
* [十. 分组可选](#ten)
* [十一. 单字符拆分（数字）](#eleven)
* [十二. 贪婪模式](#twelve)
* [十三. 非贪婪模式](#thirteen)
* [十四. 占用模式(PCRE)](#fourteen)
* [十五. |字符分界（|的作用域）](#fiveteen)
* [十六. 元字符转义](#sixteen)
* [十七. 分隔符绕过（PCRE）](#seventeen)
* [十八. 匹配溢出排除](#eighteen)
* [十九. 环视循环提取格式化](#nineteen)
* [二十. 三段论应用](#twenty)


### 一. <a name="one">分组提取/非捕获组 </a>

> 分组，是正则里一个非常重要的概念，我们需要针对某个区域提取数据，往往需要依赖分组。而分组，其实就是正则里()括住的部分。

* 分组提取 

    ```php
    需求：在分组1中匹配meta中author属性的值
    源串：
       <meta author="Zjmainstay" />
       another author="Zjmainstay too"
    预期：分组1得到Zjmainstay
    正则：'/<meta[^>]author="(.*?)".*?\/>/'
    ```
    
* 非捕获组

    >针对上面的分组，有时候，我们并不需要捕获某个分组的内容，我们可以使用非捕获组(?:表达式)，从而不捕获表达式部分内容到分组中。
    

    ```php
     源串：
     a
     ab
     abc
     abcd
     预期：匹配得到 ab 和 abcd，不包含分组1
     正则： '/^(?:\w\w)+$/m'
    ```

### <a name="two">二. 单字符或</a>

>或条件是正则使用过程中常用的概念，比如，密码由字母或数字组成，这里就用到了或条件，而且，由于字母或数字都是单个字符，因此，可以使用[a-z0-9]这样的单字符或语法实现。
 常犯错误：匹配a或b写成[a|b]，此表达式实际上表示a或b或|，在[]内部的|表示其本身，注意区分(a|b)表示a或b的写法。

```php
    需求：匹配由 A/S/D/F 4个字母(区分大小写)组成的长度为3字符串
    源串：
    ABC
    ASD
    ADS
    ASF
    BBC
    A|S
    A|D
    ASDF
    预期：以[]元字符获得3个字母的或集，匹配 ASD/ADS/ASF 3组数据
    正则：'/\b[ASDF]{3}\b/m'
```

### <a name="three">三. 多字符或</a>

>相对单字符或条件，多字符或也是很常见的，比如，我们需要匹配http或ftp两个协议头的url，就需要^(http|ftp)://.+$这样的语法来实现。

```php
    需求：匹配每行数据中以.jpg/.jpeg/.png/.gif结尾的图片名称（含后缀）
    源串：
    image.jpg
    image.jpeg
    image.png
    image.gif
    not_image.txt
    not_image.doc
    not_image.xls
    not_image.ppt
    预期：匹配 image.jpg/image.jpeg/image.png/image.gif 4个结果
    正则：'/.*?\.(jpg|jpeg|png|gif)/m'
```

### <a name="four">四. 分组引用</a>

>前面介绍了分组，那某个分组在我们匹配过程中重复出现，又该如何处理？分组引用恰恰解决这个问题。比如，匹配出现重复单词的一行数据，我们可以这么写（多行模式）：/^.*?(\b\S+\b).*?\1.*$/m，\1表示引用前面分组1中匹配到的内容，也就是重复的单词内容。

```php
    需求：匹配连续相同3次的数字
    源串：
    111
    121
    112
    222
    预期：匹配 111/222 两组数据
    正则：'/(\d)\1\1/m'
```

### <a name="five">五. 匹配换行数据</a>

>“我的正则本来好好的，突然不行了！”这个是很多正则新人遇到的问题，而这个问题，很多时候，就是因为原来正则中的.不能匹配新数据里的换行导致的。这时候，只需要把.改成[\s\S]这样的表达式就可以了。这个表达式表示空格或非空格，也就是任意字符啦。

```php
    需求：分别使用单行模式和普通模式匹配id="author"的div中数据，div标签不在同一行
    源串：
    <div id="author">
    Zjmainstay
    </div>
    预期：Zjmainstay
    正则1：'@<div[^>]id="author">(.*?)</div>@ms'
    正则2：
```

### <a name="six">六. 存在（或）</a>

* 匹配多种或条件的数据，没有特殊限制 

```php
    需求：匹配每行中包含“作者”或者“读者”的数据
    源串：
    本文的作者是Zjmainstay
    本文有很多读者
    读者可以是任何一个地方的人
    这里的任何一个地方说明读者也能在国外
    什么乱七八糟的推理
    你不匹配我，凭什么要我推荐你的博客 www.zjmainstay.cn
    预期：匹配
    本文的作者是Zjmainstay
    本文有很多读者
    读者可以是任何一个地方的人
    这里的任何一个地方说明读者也能在国外
    正则：$re = '/^.*?(作者|读者).*?$/m'
```

* 匹配多种或条件的数据，有特殊限制（不使用环视）

```php
    需求：匹配每行中“读者”在开头或结尾的数据
    源串：
    本文作者是Zjmainstay，有很多读者
    读者可以是任何一个地方的人
    这里的任何一个地方说明读者也能在国外
    预期：匹配
    本文作者是Zjmainstay，有很多读者
    读者可以是任何一个地方的人
    正则：
```

* 匹配多种或条件的数据，有特殊限制（使用环视）

```php
    需求：匹配每行中“读者”在开头或结尾的数据
    源串：
    本文作者是Zjmainstay，有很多读者
    读者可以是任何一个地方的人
    这里的任何一个地方说明读者也能在国外
    预期：匹配
    本文作者是Zjmainstay，有很多读者
    读者可以是任何一个地方的人
    正则：
```

### <a name="seven">七. 存在（与）</a>

校验密码必须包含字母、数字和特殊字符，6-16位

```php
    需求：校验密码必须包含字母、数字和特殊字符，6-16位，假定特殊字符为 -_= 三个字符
    源串：
    12345
    123456
    1234561234561234
    12345612345612345
    a1234
    a12345
    -1234
    -12345
    a-123
    a-1234
    a-1234a-1234a-12
    a-1234a-1234a-1234
    aaaaa
    aaaaaa
    -_=-_
    -_=-_=
    预期：匹配
    a-1234
    a-1234a-1234a-12
    正则：
```

### <a name="eight">八. 特殊限制（环视否定）</a>

* 使用\d{1,3}匹配1-999的数据，不能以0开头

```php
    需求：使用\d{1,3}匹配每行中1-999的数据，不能以0开头
    源串：
    1
    10
    100
    999
    1000
    01
    001
    预期：匹配
    1
    10
    100
    999
    正则：'/(?!0)(?<!\d)\d{1,3}\b/'
```
* 匹配除了span标签外的所有标签

```php
    需求：匹配除了<span>内容</span>标签外的所有<tagName>内容</tagName>格式标签
    源串：
    <div>匹配我</div>
    <span>不匹配我</span>
    <p>匹配我</p>
    <i>匹配我</i>
    预期：匹配
    <div>匹配我</div>
    <p>匹配我</p>
    <i>匹配我</i>
    正则：
```

### <a name="nine">九. 替换分组使用</a>

* 给源串每个链接加上http://www.zjmainstay.cn前缀

```php
    需求：给源串每个链接加上http://www.zjmainstay.cn前缀
    源串：
    <a id="link-1" href="/regexp-one">正则文章合集（All In One)</a>
    <a id="link-2" href="/my-regexp">正则入门教程</a>
    <a id="link-3" href="/deep-regexp">正则高级教程</a>
    <a id="link-4" href="/regexp-lookaround">正则环视详解</a>
    <a id="link-5" href="/php-curl">PHP cURL应用</a>
    预期：替换得到
    <a id="link-1" href="http://www.zjmainstay.cn/regexp-one">正则文章合集（All In One)</a>
    <a id="link-2" href="http://www.zjmainstay.cn/my-regexp">正则入门教程</a>
    <a id="link-3" href="http://www.zjmainstay.cn/deep-regexp">正则高级教程</a>
    <a id="link-4" href="http://www.zjmainstay.cn/regexp-lookaround">正则环视详解</a>
    <a id="link-5" href="http://www.zjmainstay.cn/php-curl">PHP cURL应用</a>
    查找：
    替换：
```
* 将每行数据格式化为一条SQL语句

```php
    需求：将每行特定格式数据格式化为SQL语句
    源串：
    1 2017-04-11 Zjmainstay
    2 2017-04-12 Nobody
    3 2017-04-13 Somebody
    预期：替换得到
    INSERT INTO table_log(`id`, `created_at`, `author`) values('1', '2017-04-11', 'Zjmainstay');
    INSERT INTO table_log(`id`, `created_at`, `author`) values('2', '2017-04-12', 'Nobody');
    INSERT INTO table_log(`id`, `created_at`, `author`) values('3', '2017-04-13', 'Somebody');
    查找：
    替换：
```

### <a name="ten">十. 分组可选</a>

* 分组可选

```php
    需求：判断如果单词以A开头，匹配Apple；如果单词以B开头，匹配Banana；否则匹配Empty
    源串：
    Angle
    Apple
    Banana
    Best
    Empty
    预期：匹配
    Apple
    Banana
    Empty
    正则：
```
* 分组可选与分组引用

```php
    需求：匹配html标签的属性值，属性值可以由双引号、单引号、无单双引号定界
    源串：
    <div id="I'm Zjmainstay" class="name" data-year=2017 age='27'>
    预期：分组匹配
    I'm Zjmainstay
    author
    2017
    27
    正则：
```
### <a name="eleven">十一. 单字符拆分（数字）</a>

匹配0.00-100.00的数值，可以有0-2位小数

```php
    需求：匹配0.00-100.00的数值，可以有0-2位小数，不能以小数点结尾，不能以2个以上的0开头
    思路：(100|10-99|0-9) + 0-2小数位 + 排除小数点结尾、2个以上0开头的情况
    源串：
    0
    1
    0.0
    0.00
    9.00
    18.00
    27.0
    36.00
    45.00
    54.00
    63.00
    72.00
    81.00
    90.00
    99.99
    100.00
    0.
    001
    100.01
    100.001
    101
    预期：匹配0.00~100.00
    正则：
```

### <a name="twelve">十二. 贪婪模式</a>

* 匹配链接中的文件名

```php
    需求：利用贪婪模式，分组1得到每行链接中的文件名
    源串：
    http://localhost.com/a/b/c/d/file1.txt
    https://localhost.com/a/b/file2long.jpg
    预期：分组0匹配行数据，分组1匹配文件名
    file1.txt
    file2long.jpg
    正则：
```
* 限定字符贪婪优化匹配性能

```php
    需求：匹配div id="author"的标签内容
    源串：
    <div id="author" class="author-text something-useless">Zjmainstay</div>
    预期：利用贪婪模式去掉div中的噪点（无关数据），分组1匹配到Zjmainstay
    正则：
```

### <a name="thirteen">十三. 非贪婪模式</a>

>贪婪模式，正则会优先尽可能少地匹配能匹配到的内容。当剩余正则匹配剩余部分字符（源串）但无法满足匹配时，非贪婪部分继续匹配更多内容，尝试满足剩余部分字符的匹配。

匹配p标签内容

```php
    需求：匹配p标签内容
    源串：
    <p>内容1</p><p>内容2</p>
    预期：
    在分组1中匹配到内容1和内容2
    正则：
```

### <a name="fourteen">十四. 占用模式(PCRE)</a>

>贪婪模式后再加一个+量词，如.++，效果是贪婪而且不回溯。
 暂时没有想到应用场景。
 
### <a name="fiveteen">十五. |字符分界（|的作用域）</a>

>|作为或条件分隔符，它的分隔区间常常存在误用。在使用|字符的过程中，我们常常需要结合()来对它进行限定。如，^([0-9]+|[a-z]+)$表示纯数字或纯字母，如果没有()，那它又是另一种意思了。^[0-9]+|[a-z]+$等价于^[0-9]+或[a-z]+$，因此，它表示数字开头或者字母结尾，跟我们的需求有了很大的差别。

|字符分界

```php
    需求：在分组1中匹配css或script的链接
    源串：
    <script src="main.min.js" type="text/javascript"></script>
    <link rel="stylesheet" type="text/css" href="main.css">
    预期：
    main.min.js
    main.css
    正则：
```

### <a name="sixteen">十六. 元字符转义</name>

>元字符，指正则中有特殊意义的字符，如.表示匹配除了换行符以外的任意字符，这个.就是元字符。在正则书写过程中，如果我们真的要匹配这个.，就需要对它进行转义，而不是让它使用正则的含义，比如，匹配域名里的.，我们就要写成/zjmainstay\.cn/这样的正则。

元字符转义

```php
    需求：表达式格式固定，提取其中的数值
    源串：
    (20+170)-5*1/5=?
    预期：
    A:20
    B:170
    C:5
    D:1
    E:5
    F:?
    正则：
    替换：
```

### <a name="seventeen">十七. 分隔符绕过（PCRE）</a>

>有时候，如果该语言支持多种分隔符，在写正则的过程中通常会通过规避分隔符的方式，减少对分隔符的转义，让正则看起来更清晰，写起来更舒服，当然，js中是不支持的。

分隔符绕过

```php
    需求：在不对/转义的情况下匹配p标签内容
    源串：
    <p>内容1</p><p>内容2</p>
    预期：
    在分组1中匹配到内容1和内容2
    正则：<p>([^<]+)</p>@m
```

### <a name="eighteen">十八. 匹配溢出排除</a>

>匹配溢出，这不是一个术语名词，是我自己的叫法，主要指正则匹配内容超出了我们预期，导致匹配得到非预期的结果。

* div标签匹配溢出

```php
    需求：匹配内容为数字的div
    源串：
    <div class="aaa bbb">ABC</div><div class="bbb ccc">123</div>
    预期：
    <div>123</div>
    错误正则：/<div.*?>\d+<\/div>/
    正则：/<div[^>]*>\d+<\/div>/
```
* 多字符排除

```php
    需求：匹配不包含某个单词或词语的内容
    源串：
    http://www.zjmainstay.cn
    http://www.baidu.com
    http://www.qq.com
    预期：
    http://www.zjmainstay.cn
    http://www.qq.com
    正则：/http:\/\/www\.[^baidu].*/m
    
    需求：匹配不包含某个单词或词语的内容
    源串：
    A("Excalibur", "誓约胜利之剑", LONG_SWORD, (SPFX_NOGEN | SPFX_RESTR | SPFX_SEEK | SPFX_DEFN | SPFX_INTEL | SPFX_SEARCH), 
      0, 0, PHYS(5, 10), DRLI(0, 0), NO_CARY, 0, A_LAWFUL, PM_KNIGHT, NON_PM, 4000L, NO_COLOR);
    /*
     * Stormbringer only has a 2 because it can drain a level,
     * providing 8 more.
     */
    A("Stormbringer", "兴风者", RUNESWORD,
      (SPFX_RESTR | SPFX_ATTK | SPFX_DEFN | SPFX_INTEL | SPFX_DRLI), 0, 0,
      DRLI(5, 2), DRLI(0, 0), NO_CARY, 0, A_CHAOTIC, NON_PM, NON_PM, 8000L,
      NO_COLOR);
    /*
     * Mjollnir will return to the hand of the wielder when thrown
     * if the wielder is a Valkyrie wearing Gauntlets of Power.
     */
    A("Mjollnir", "雷神之锤", WAR_HAMMER, /* Mjo:llnir */
      (SPFX_RESTR | SPFX_ATTK), 0, 0, ELEC(5, 24), NO_DFNS, NO_CARY, 0,
      A_NEUTRAL, PM_VALKYRIE, NON_PM, 4000L, NO_COLOR);
    A("Cleaver", "撕裂者", BATTLE_AXE, SPFX_RESTR, 0, 0, PHYS(3, 6), NO_DFNS, NO_CARY, 0, A_NEUTRAL, PM_BARBARIAN, NON_PM, 1500L, NO_COLOR);
    /*
     * Grimtooth glows in warning when elves are present, but its
     * damage bonus applies to all targets rather than just elves
     * (handled as special case in spec_dbon()).
     */
    A("Grimtooth", "邪兽之牙", ORCISH_DAGGER, (SPFX_RESTR | SPFX_WARN | SPFX_DFLAG2),
      0, M2_ELF, PHYS(2, 6), NO_DFNS,
      NO_CARY, 0, A_CHAOTIC, NON_PM, PM_ORC, 300L, CLR_RED);
    /*
     * Orcrist and Sting have same alignment as elves.
     *
     * The combination of SPFX_WARN+SPFX_DFLAG2+M2_value will trigger
     * EWarn_of_mon for all monsters that have the M2_value flag.
     * Sting and Orcrist will warn of M2_ORC monsters.
     */
    A("Orcrist", "杀兽剑", ELVEN_BROADSWORD, (SPFX_WARN | SPFX_DFLAG2), 0, M2_ORC, PHYS(5, 0), 
      NO_DFNS, NO_CARY, 0, A_CHAOTIC, NON_PM, PM_ELF, 2000L, CLR_BRIGHT_BLUE); /* bright blue is actually light blue */
    预期：
    Excalibur=誓约胜利之剑
    Stormbringer=兴风者
    Mjollnir=雷神之锤
    Cleaver=撕裂者
    Grimtooth=邪兽之牙
    Orcrist=杀兽剑
    查找：
    替换：
```

### <a name="nineteen">十九. 环视循环提取格式化</a>

>在数据处理过程中，经常遇到一些格式化处理，比如简单地将一批数据格式化为SQL（参考9.2），还有复杂的需要对一行数据的某部分进行循环提取，然后格式化为特定格式。

```php
    需求：循环提取每行数据的分支部分和固定部分，格式化为特定格式
    源串：
    BBB|CCC|DDD=AAA
    FFF|GGG|HHH|III|JJJ|KKK=EEE
    预期：
    BBB=AAA
    CCC=AAA
    DDD=AAA
    FFF=EEE
    GGG=EEE
    HHH=EEE
    III=EEE
    JJJ=EEE
    KKK=EEE
    查找：\|(?=.*(=.*))
    替换：$1\n
```
### <a name="twenty">二十. 三段论应用</a>

>三段论:定锚点，去噪点，取数据。
 **锚点**，在正则中指^、$、\b这类零宽的位置，这里做了衍生，指能够唯一确定我们目标数据位置的参照点，比如author=lalala，我们要匹配author属性的数据，则author=就是我们的参照点，通过它，我们能快速写出提取author属性的数据的正则：author=(.+)。
 **噪点**，就是对我们提取数据产生干扰的无关数据，我们在做正则匹配提取数据的过程中，可以选择性的忽略它们。当然，这里的忽略不是指不需要对它们做匹配，而是不需要对它们做精确匹配。
 **数据**，这个当然是指我们需要提取的内容了，如上面锚点举例，我们通过author=(.+)的(.+)对lalala部分数据进行了提取，因此，匹配结果的分组1（程序语言中的数组下标1）中，就能得到我们的结果。而对于多个数据的提取，如噪点举例，我们只需要针对数据部分进行多个分组（括号）的提取即可。
 分组的计数，一般可以数左括号，排除环视和非捕获组的左括号，从1开始，依次加1递增，1,2,3,4....n，不同语言最大分组个数不同，大家在使用过程中自行留意，不过一般用不了那么多分组。
 理解了三段论的概念，我们在写正则的过程中，只需要将源串进行分割划分，根据目标数据确定锚点，过滤噪点，提取数据，就能得到我们想要的正则了。