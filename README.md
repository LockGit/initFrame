### initFrame 一个自实现的php框架
```
声明：请无视代码之烂（initFrame为很久以前学习php所留下来的东西）

如果WooYun还没有关闭,应该可以看到我提交的最后一个ThinkPHP框架漏洞，
ThinkPHP官方在1~2天内便修复了，在此之前就已经有人对ci，yii之类的
frame做过分析。那篇文章详细分析了漏洞的成因也详细叙述了框架的执行过程,
WooYun关闭以后发现文章已经找不到了。

想在github上再次做个整理,以便永久保存,initFrame只描述PHP相关的东西。


initFrame实现了那些功能？
1,基础的组织代码的功能(MVC)
2,自解析的模板引擎(自解析：变量，数组，if，for，注释，等模板语法)
3,支持缓存
---
说initFrame是一个自实现的php框架并不是说这个框架完全是自己造出来的，
当再次整理这个的时候，发现时间已经过去很久很久了。如果在不整理下，或许
就永远懒的整理了。initFrame为初学php时在网上翻阅了大量资料,参考前人的
思想最终形成的一个低级别的php框架，有很多实现不合理的地方，仅当做学习使用。

执行过程图：
```
![](https://github.com/LockGit/initFrame/blob/master/doc/initFrame.png)

```
着重说明view加载模板文件过程
核心解析类文件有(实现了模板引擎的功能)：
	initFrame/includes/Parse.class.php
	initFrame/includes/Template.class.php

1,首先通过解析类解析tpl模板文件,其实就是通过一系列的正则匹配替换
tpl文件中的一些字符,生成可执行的php文件,这里叫做编译文件

2,编译文件生成后,也就是纯php文件，php解析器开始解析这个纯php文件
并生成纯静态文件(当缓存开启)

3,当修改了index.tpl模板文件,程序会把index.tpl文件的最后修改时间(t1)与
index.tpl.php文件的最后修改时间(t2)进行比较，当t1>t2,那么重新生成
index.tpl.php文件和index.tpl.html文件

4,如果开启了缓存，那么直接加载缓存后的html文件，即cache目录中的静态文件
```
### 测试
```
cd initFrame && php -S 0:8888
访问：http://127.0.0.1:8888/index.php
response:
index.tpl文件

首页
音乐
歌曲
舞蹈
hello
由系统变量设置的分页:15

$name的值：Lock
设置if语句

No
设置Foreach语句

0 => 1 
1 => 2 
2 => 3 
3 => 4 
4 => 5 
5 => 6 
6 => 7 
文件引入:mmaaabbcc456123xzhes
---
访问：http://127.0.0.1:8888/test.php
response:
test.tpl文件

test val is :hello world

name is: Lock

```