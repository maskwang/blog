---
title: PHPRPC初探
tags:
  - php
  - rpc
id: 647
categories:
  - Php
  - 编程语言
date: 2012-09-11 15:34:14
---

 phprpc，一种跨平台、跨语言的简单传输协议。方便实用。
分为客户端和服务端，从hello world说起：

服务端：server.php<!--more-->
[php]
&lt;?php
/**
 * phprpc服务端
 * @author maskwang&lt;mask.wang.cn@gmail.com&gt;
 * @version v0.1  12-9-11 下午2:38
 */
include 'phprpc/phprpc_server.php';
$server = new PHPRPC_Server();

#测试函数
function HelloWorld()
{
    return 'Helllo World!';
}
$server-&gt;add('HelloWorld');

#测试静态方法
class Mask
{
    static function Wang()
    {
        return 'Mask Wang';
    }

}
$server-&gt;add('Wang', 'Mask');
$server-&gt;start();
[/php]

客户端：client.php
[php]
&lt;?php
/**
 * phprpc客户端
 * @author maskwang&lt;mask.wang.cn@gmail.com&gt;
 * @version v0.1  12-9-11 下午2:41
 */
include 'phprpc/phprpc_client.php';
$client = new PHPRPC_Client('http://127.0.0.1/test/rpc/server.php');
echo $client-&gt;HelloWorld();
      echo '&lt;hr /&gt;';
echo $client-&gt;Wang();
[/php]