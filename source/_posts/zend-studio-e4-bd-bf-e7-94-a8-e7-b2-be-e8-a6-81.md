---
title: Zend Studio 使用精要
id: 94
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:41:42
tags:
---

Zend Studio 使用精要
编写环境：
Zend Studio 5.1.0
PHP4 &amp; PHP5
1\. 版本控制
Zend Studio 4只支持CVS, Zend Studio 5 开始加入subversion的支持，后者的使用比较简单，本文以后者与Zend Studio集成使用为例做说明。
Zend Studio默认使用CVS，可在“工具”-&gt;“首选项”-&gt;“source control”中选择Subversion即可。
配 置Zend Studio客户端使用SVN：
打开“工具”-&gt;“subversion”-&gt;”checkout“，显示如下对话框：
Module ULR 指要下载的源程序在源码库的位置，如图所示
工作目录是下载到本机的程序存放位置，如图所示，如果所填目录不存在，则程序自动创建。
用户名密码如果不需要的时候默认为空。
上面菜单是在Zend Studio代码编辑区域捕捉的。
Subversion菜单命令说明：
Update : 将svn 源码库端文件同步到本地的工作拷贝。
Commit: 提交当前工作拷贝的更改。这个地方是有可能出现代码冲突的。最安全的解决方法，先update一下，再修改程序并Commit。
Add :将当前文件添加到版本控制库中。原来该版本不处于版本控制之下。比如新建立的一个程序或者文件。
Delete: 将当前文件从版本控制库中删除，脱离svn版本控制。
Revert : 取消当前文件的所有的本地编辑。并且解决所有的冲突状态。
Resove : 删除工作拷贝文件或目录的“冲突”状态。
Status: 查看当前工作拷贝文件和目录的状态。
Diff : 比较当前文件与源码库中相应文件的不同。
Log : 当前文件的所有修改记录，从创建开始的每一次修改都能显示出来。

注意：上面的命令也可以在Zend Studio 左侧的项目区域对多个文件或文件夹同时操作。

在修改完成之后，可以到程序运行服务器的 项目目录下svn update一下，就可得到最新的程序。

Svn高级操作：
a. 解决冲突（合并别人的修改）

b. 分支与合并
2\. 程序调试
Zend Studio 支持两种调试方式：内部调试器，服务器端调试器

内部调试器：使用 本地Zend Studio 自带的PHP4/5引擎执行程序。
服务器端调试器：使用服务器上的PHP环境来执行程序。

因为服务 器一般为linux，而我们开发使用一般为windows，那么PHP环境肯定有所不同，选择使用服务器端调试器更合理。

下面就以服务器 端调试为例来说明问题：

a. 配置Zend Studio支持服务器调试
打开 “工具”-&gt;“首选项”-&gt;“调试”，显示如下对话框：
选择“服务器”调试方式，并在调试服务器URL中填写正确的URL即可。

现 在在测试服务器上安装有PHP4及PHP5两个版本的调试器，
PHP4 对应URL为：http://192.168.3.33
PHP5 对应URL为：http://192.168.3.33:81
其他的设置为默认值即可。
测试调试器的配置是否正确：
打开：“工具”-&gt;“检查debug server连接”进行测试。

b. 调试命令说明

“添加监视点。。。 “：即添加你关心的变量，它在单独的窗口中显示它的值。

“调试URL。。。”：单步执行给定的URL

“概要文件URL。。。”：对给定的URL的程序执行情况做分析统计，包括程序中各函数的调用，效率，等。
<table width="360" border="1" cellspacing="0" cellpadding="0" align="center">
<tbody>
<tr>
<td align="center" width="60">![](http://hiphotos.baidu.com/wangfengkun/pic/item/d56285355950312c90ef39af.jpg)</td>
<td>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td align="center" width="40">文件:</td>
<td>zend studio使用手册.rar</td>
</tr>
<tr>
<td align="center" width="40">大 小:</td>
<td>125KB</td>
</tr>
<tr>
<td align="center" width="40">下 载:</td>
<td>[下载](http://blogimg.chinaunix.net/blog/upfile/070125162519.rar)</td>
</tr>
</tbody>
</table>
</td>
</tr>
</tbody>
</table>
c. 调试示例

d. 其他
在调试模式下，将鼠标放到Zend Studio代码编辑区域的变量上，可查看变量当前值。

在 运行时手动改变某个变量的值，在调试窗口的监视点里修改变量的值。

调试窗口的缓冲区TAB里显示的是某时刻在PHP输出缓冲区中的数据， 这个时候浏览器可能还没有输出。

3．