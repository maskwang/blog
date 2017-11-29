---
title: 'PHPRPC for PHP '
id: 323
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2011-09-15 22:18:03
tags:
---

PHPRPC 是一个轻型的、安全的、跨网际的、跨语言的、跨平台的、跨环境的、跨域的、支持复杂对象传输的、支持引用参数传递的、支持内容输出重定向的、支持分级错误处理的、支持会话的、面向服务的高性能远程过程调用协议。
下载地址:http://www.phprpc.org/zh_CN/download/
该版本直接解压后就可以使用，其中
属于公共文件。不论是客户端还是服务器端都需要这些文件。
是客户端文件，如果你只需要使用客户端，那么只要有上面那些公共文件和这个文件就可以使用了，使用时，直接在你的程序中包含 phprpc_client.php 就可以，公共文件不需要单独包含。
这三个文件是服务器端需要的文件。
其中 dhparams 目录中包含的是加密传输时用来生成密钥的参数
dhparams.php 是用来读取 dhparams 目录中文件的类。
phprpc_server.php 是服务器端，如果你要使用 PHP 来发布 PHPRPC 服务，只需要包含这个文件就可以了。公共文件和 dhparams.php 都不需要单独包含。
PHP 4.3+、PHP 5、PHP 6
客户端要求开启 socket 扩展。
服务器端需要有 IIS、Apache、lighttpd 等可以运行 PHP 程序的 Web 服务器。
如果服务器端需要加密传输的能力，必须要保证 session 配置正确。
&lt;?php
include('php/phprpc_server.php'); //加载文件
function hello($name) {
return'Hello ' . $name;
}
$server = new PHPRPC_Server(); //创建服务端
$server-&gt;add(array('hello', 'md5', 'sha1')); //数组形式一次注册多个函数
$server-&gt;add('trim'); //单一注册
$server-&gt;start(); //开启服务
?&gt;
&lt;?php
include ("php/phprpc_client.php"); //加载文件
$client = new PHPRPC_Client('http://127.0.0.1/server.php'); //创建客户端 并连接服务端文件
echo$client-&gt;Hello("word"); //调用方法 返回 hello word
?&gt;
-------------------------------------------------- --------------------------------------------------- ------------------------------
服务端其他说明:
&lt;?php
include('php/phprpc_server.php'); //加载文件
function hello($name) {
return'Hello ' . $name;
}
class Example1 {
staticfunction foo() {
return'foo';
}
function bar() {
return'bar';
}
}
$server = new PHPRPC_Server(); //创建服务端
$server-&gt;add('foo', 'Example1'); //静态方法直接调用
$server-&gt;add('bar', new Example1()); //非静态方法 需要实例化
//注册别名调用
$server-&gt;add('hello', NULL, 'hi'); //第三参数是函数的别名 客户端通过别名来调用函数
$server-&gt;add('foo', 'Example1', 'ex1_foo');
$server-&gt;add('bar', new Example1(), 'ex1_bar');
$server-&gt;setCharset('UTF-8'); //设置编码
$server-&gt;setDebugMode(true); //打印错误
$server-&gt;setEnableGZIP(true); //启动压缩输出虽然可以让传输的数据量减少，但是它会占用更多的内存和 CPU，因此它默认是关闭的。
$server-&gt;start(); //开启服务
?&gt;
-------------------------------------------------- --------------------------------------------------- ---------------------------
客户端其他说明:
&lt;?php
include ("php/phprpc_client.php");
$client = new PHPRPC_Client();
$client-&gt;useService('http://127.0.0.1/server.php'); //远程调用地址
$client-&gt;setKeyLength(1000); //密钥长度
$client-&gt;setEncryptMode(3); //加密等级0-3
$client-&gt;setCharset('UTF-8'); //设置编码
$client-&gt;setTimeout(10); //设置超时时间
echo$client-&gt;hi('PHPRPC'), "\r\n"; //调用函数
echo$client-&gt;getKeyLength(), "\r\n"; //下面是返回值
echo$client-&gt;getEncryptMode(), "\r\n";
echo$client-&gt;getCharset(), "\r\n";
echo$client-&gt;getTimeout(), "\r\n";
?&gt;
-------------------------------------------------- --------------------------------------------------- ----------------------
关于session
&lt;?php
include('php/phprpc_server.php');
class ExampleCounter {
function ExampleCounter() {
if (!isset($_SESSION['count'])) {
$_SESSION['count'] = 0;
}
}
function inc() {
$_SESSION['count'] += 1;
}
functioncount() {
return$_SESSION['count'];
}
}
$server = new PHPRPC_Server();
$server-&gt;add(array('inc', 'count'), new ExampleCounter());
$server-&gt;start();
?&gt;
&lt;?php
include("php/phprpc_client.php");
$client = newPHPRPC_Client();
$client-&gt;useService('http://127.0.0.1/1.php');
$client-&gt;setTimeout(10);
echo $client-&gt;inc();
echo $client-&gt;count();
echo $client-&gt;inc();
echo $client-&gt;count();
?&gt;
每次刷新都是新建的client 服务端并不能识别.