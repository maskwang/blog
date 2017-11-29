---
title: Shell中的grep、awk和sed的常用命令和语法
id: 435
categories:
  - Linux
  - 技术专栏
date: 2011-11-02 11:10:15
tags:
---

<span style="color: #ff0000; font-size: x-large;">Grep的常用命令语法</span>

1\. 双引号引用和单引号引用
在g r e p命令中输入字符串参数时，最好将其用双引号括起来。例如：”m y s t r i n g”。这样做有两个原因，一是以防被误解为 s h e l l命令，二是可以用来查找多个单词组成的字符串，例如：”jet plane”，如果不用双引号将其括起来，那么单词 p l a n e将被误认为是一个文件，查询结果将返回”文件不存在”的错误信息。
在调用变量时，也应该使用双引号，诸如： g r e p”$ M Y VA R”文件名，如果不这样，将
没有返回结果。
在调用模式匹配时，应使用单引号.[root@mypc ]# echo `grep 123 111.txt`  (#注意是反单引号)

2\. 常用的g r e p选项有：
-c   只输出匹配行的计数。
-i   不区分大小写（只适用于单字符）。
-h   查询多文件时不显示文件名。
-l   查询多文件时只输出包含匹配字符的文件名。
-n   显示匹配行及行号。
-s   不显示不存在或无匹配文本的错误信息。
-v   显示不包含匹配文本的所有行。

3\. 特殊的–在多个文件中进行查询
$ grep “sort”*.doc     ( #在当前目录下所有. d o c文件中查找字符串”s o r t”)

$ grep “sort it” *      (#或在所有文件中查询单词”sort it”)
接下来的所有示例是指在单个文件中进行查询
4\. 行匹配
$ grep -c “48″ data.f
$ 4                      (#g r e p返回数字4，意义是有4行包含字符串”4 8″。)
$ grep “48″ data.f          (#显示包含”4 8″字符串的4行文本)

5\. 显示满足匹配模式的所有行行数：
[root@mypc oid2000]# grep -n 1234 111.txt
1:1234
3:1234ab

6\. 精确匹配
[root@mypc oid2000]# grep “1234\&gt;” 111.txt
1234

7\. 查询空行，查询以某个条件开头或者结尾的行。
结合使用^和$可查询空行。使用- n参数显示实际行数
[root@mypc oid2000]# grep -n “^$” 111.txt    (返回结果 2:   #说明第二行是空行)
[root@mypc oid2000]# grep -n “^abc” 111.txt （#查询以abc开头的行）
[root@mypc oid2000]# grep -n “abc$” 111.txt  (#查询以abc结尾的行)

8\. 匹配特殊字符,查询有特殊含义的字符，诸如$ . ‘ ” * [] ^ | \ + ? ,必须在特定字符前加\。
[root@mypc oid2000]# grep  “\.” 111.txt  (#在111.txt中查询包含”.”的所有行)
[root@mypc oid2000]# grep  “my\.conf”  111.txt (#查询有文件名my. c o n f的行)

9\. 目录的查询
[root@mypc oid2000]# ls -l |grep “^d”      (#如果要查询目录列表中的目录)
[root@mypc oid2000]# ls -l |grep “^d[d]”    (#在一个目录中查询不包含目录的所有文件)
[root@mypc]# ls -l |grpe “^d…..x..x” (#查询其他用户和用户组成员有可执行权限的目录集合)

&nbsp;

&nbsp;

<span style="color: #ff0000; font-size: x-large;">Awk的常用命令语法</span>

awk命令擅长格式化报文或从一个大的文本文件中抽取数据包,下面是该命令的基本语法
awk [-F filed-separator] “commands” input-file(s)
[ - F域分隔符]是可选的,a w k使用空格作为缺省的域分隔符，如果在要处理的文件中是以冒号作为分割域的(如passwd文件),则在处理的时候要这样指明 awk -F: command input-file(s)

1.1域和记录
a w k执行时，其浏览域标记为$ 1，$ 2 . . . $ n。这种方法称为域标识。使用$ 1 , $ 3表示参照第1和第3域，注意这里用逗号做域分隔。如果希望打印一个有 5个域的记录的所有域，不必指明 $ 1 , $ 2 , $ 3 , $ 4 , $ 5，可使用$ 0，意即所有域。

1.2保存a w k输出
$ awk ‘{print $0}’ input-files &gt; out-files    (#重定向保存输出)
$ awk ‘{print $0}’ input-files | tee out-files  (#使用t e e命令，输出到文件的同时输出到屏幕)

1.3 常用的awk命令举例
[root@mypc /]# awk ‘$0 ~ /user/’ /etc/passwd  (#如果某域含有user就将该行打印出来)
rpc:x:32:32:Portmapper RPC user:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
[root@mypc /]# awk ‘/user/’ /etc/passwd      (#同上)
[root@mypc /]# awk -F: ‘{if ($5 ~ /user/) print $0}’ /etc/passwd (#如第五域有user则输出该行)
rpc:x:32:32:Portmapper RPC user:/:/sbin/nologin
[root@mypc /]# ifconfig | awk ‘/inet/{print $2}’  (#从ifconfig的输出中抽取含inet的行并打印第二域)
[root@mypc /]# ifconfig | awk ‘/inet/{print $2}’ | awk -F: ‘{print $2}’ (#在上面的基础上再抽取，这个命令可以让你直接得到本机的ip地址)

&nbsp;

&nbsp;

<span style="color: #ff0000; font-size: x-large;">Sed的常用命令语法</span>
Sed是一个非交互性文本流编辑器。它编辑文件或标准输入导出的文本拷贝。

1．行的匹配
[root@mypc /]# sed -n ‘2p’ /etc/passwd  打印出第2行
[root@mypc /]# sed -n ‘1,3p’ /etc/passwd 打印出第1到第3行
[root@mypc /]# sed -n ‘$p’ /etc/passwd   打印出最后一行
[root@mypc /]# sed -n ‘/user/’p /etc/passwd 打印出含有user的行
rpc:x:32:32:Portmapper RPC user:/:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
[root@mypc /]# sed -n ‘/\$/’p /etc/passwd  打印出含有$元字符的行，$意为最后一行

2．插入文本和附加文本(插入新行)
[root@mypc /]# sed -n ‘/FTP/p’ /etc/passwd  打印出有FTP的行
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
[root@mypc /]# sed ‘/FTP/ a\ 456′ /etc/passwd 在含有FTP的行后面新插入一行，内容为456
[root@mypc /]# sed ‘/FTP/ i\ 123′ /etc/passwd在含有FTP的行前面新插入一行，内容为123
[root@mypc /]# sed ‘/FTP/ i\ “123″‘ /etc/passwd在含有FTP的行前面新插入一行，内容为”123″
[root@mypc /]# sed ‘5 a\ 123′ /etc/passwd         在第5行后插入一新行，内容为123
[root@mypc /]# sed ‘5 i\ “12345″‘ /etc/passwd   在第5行前插入一新行，内容为”12345″

3．删除文本
[root@mypc /]# sed ‘1d’ /etc/passwd  删除第1行
[root@mypc /]# sed ‘1,3d’ /etc/passwd  删除第1至3行
[root@mypc /]# sed ‘/user/d’ /etc/passwd  删除带有user的行

4． 替换文本,替换命令用替换模式替换指定模式，格式为：
[ a d d r e s s [，address]] s/ pattern-to-find /replacement-pattern/[g p w n]
[root@mypc /]# sed ’s/user/USER/’ /etc/passwd     将第1个user替换成USER,g表明全局替换
[root@mypc /]# sed ’s/user/USER/g’ /etc/passwd    将所有user替换成USER
[root@mypc /]# sed ’s/user/#user/’ /etc/passwd    将第1个user替换成#user,如用于屏蔽作用
[root@mypc /]# sed ’s/user//’ /etc/passwd         将第1个user替换成空
[root@mypc /]# sed ’s/user/&amp;11111111111111/’ /etc/passwd  如果要附加或修改一个很长的字符串，可以使用（ &amp;）命令，&amp;命令保存发现模式以便重新调用它，然后把它放在替换字符串里面，这里是把&amp;放前面
[root@mypc /]# sed ’s/user/11111111111111&amp;/’ /etc/passwd  这里是将&amp;放后面

5\. 快速一行命令
下面是一些一行命令集。（[ ]表示空格，[ ]表示t a b键）
‘s / \ . $ / / g’ 删除以句点结尾行
‘-e /abcd/d’ 删除包含a b c d的行
‘s / [ ] [ ] [ ] * / [ ] / g’ 删除一个以上空格，用一个空格代替
‘s / ^ [ ] [ ] * / / g’ 删除行首空格
‘s / \ . [ ] [ ] * / [ ] / g’ 删除句点后跟两个或更多空格，代之以一个空格
‘/ ^ $ / d’ 删除空行
‘s / ^ . / / g’ 删除第一个字符
‘s /COL \ ( . . . \ ) / / g’ 删除紧跟C O L的后三个字母
‘s / ^ \ / / / g’ 从路径中删除第一个\
‘s / [ ] / [ ] / / g’ 删除所有空格并用t a b键替代
‘S / ^ [ ] / / g’ 删除行首所有t a b键
‘s / [ ] * / / g’ 删除所有t a b键
如果使用s e d对文件进行过滤，最好将问题分成几步，分步执行，且边执行边测试结果。
经验告诉我们，这是执行一个复杂任务的最有效方式。