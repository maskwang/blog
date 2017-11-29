---
title: php curl常用函数
id: 282
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2011-08-25 22:28:38
tags:
---

&nbsp;

php curl常用于：GET，POST，HTTP验证，**302重定向**，设置cURL的代理。

1、开启PHP的cURL功能

在Windows平台下，或者使用xampp之类的集成服务器的程序，会非常简单，你需要改一改你的**php.ini**文件的设置，找到php_curl.dll，并取消前面的分号注释就行了。如下所示：

//取消注释，开启cURL功能
extension=php_curl.dll

在**[Linux](http://www.js8.in/tag/linux)**下面，那么，你需要重新编译你的PHP了，编辑时，你需要打开编译参数——在configure命令上加上“–with-curl” 参数。

2、使用cURL来GET数据

cURL最简单最常用的采用**GET**来获取网页内容的**PHP函数**
function getCURL($url){
$curl = curl_init();
curl_setopt($curl, CURLOPT_URL, $url);
curl_setopt($curl, CURLOPT_HEADER, 0);
curl_setopt($curl, CURLOPT_TIMEOUT, 3);//超时时间
curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
$data = curl_exec($curl);
curl_close($curl);
return $data;
}

3、使用cURL来POST数据

当我们需要对cURL请求的页面采用**POST**的请求方式时，我们使用下面的**PHP函数**
function _curl_post($url, $vars) {
$ch = curl_init();
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $vars);
$data = curl_exec($ch);
curl_close($ch);
if ($data)
return $data;
else
return false;
}

4、使用cURL，需要HTTP服务器认证

当我们请求地址需要加上身份验证，即**HTTP服务器认证**的时候，我们就要使用下面的函数了，对于cURL中**GET方法**使用验证也是采用相同的方式。
function postCurlHTTP($url, $str) {
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_USERPWD, “验证的用户名:密码”);
curl_setopt($ch, CURLOPT_POSTFIELDS, $str);
$data = curl_exec($ch);
$Headers = curl_getinfo($ch);
if ($Headers['http_code'] == 200) {
return $data;
} else {
return false;
}
}

5、使用cURL获取302重定向的页面

下面函数$data为重定向后页面的内容，这里我们写一个简单的**cURL POST**的**302重定向**后返回重定向页面URL的函数，有时候返回页面的URL更加重要。
function _curl_post_302($url, $vars) {
$ch = curl_init();
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1); // 302 redirect
curl_setopt($ch, CURLOPT_POSTFIELDS, $vars);
$data = curl_exec($ch);
$Headers = curl_getinfo($ch);
curl_close($ch);
if ($data&amp;&amp;$Headers)
return s$Headers["url"];
else
return false;
}

6、给cURL加个代理服务器

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, ‘[http://www.js8.in](http://www.js8.in/)‘);
curl_setopt($ch, CURLOPT_HEADER, 1);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_HTTPPROXYTUNNEL, 1);
curl_setopt($ch, CURLOPT_PROXY, ‘代理服务器地址（[www.js8.in](http://www.js8.in/)）:端口’);
curl_setopt($ch, CURLOPT_PROXYUSERPWD, ‘代理用户:密码’);
$data = curl_exec();
curl_close($ch);

7、一个cURL简单的类

&lt;?php
/*
Sean Huber**CURL**library

This library is a basic implementation of CURL capabilities.
It works in most modern versions of IE and FF.

==================================== USAGE ====================================
It exports the CURL object globally, so set a callback with setCallback($func).
(Use setCallback(array(’class_name’, ‘func_name’)) to set a callback as a func
that lies within a different class)
Then use one of the CURL request methods:

**get($url)**;
**post($url, $vars)**; vars is a urlencoded string in query string format.

Your callback function will then be called with 1 argument, the response text.
If a callback is not defined, your request will return the response text.
*/

class CURL {
var $callback = false;

function setCallback($func_name) {
$this-&gt;callback = $func_name;
}

function doRequest($method, $url, $vars) {
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_HEADER, 1);
curl_setopt($ch, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_COOKIEJAR, ‘cookie.txt’);
curl_setopt($ch, CURLOPT_COOKIEFILE, ‘cookie.txt’);
if ($method == ‘POST’) {
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $vars);
}
$data = curl_exec($ch);
curl_close($ch);
if ($data) {
if ($this-&gt;callback)
{
$callback = $this-&gt;callback;
$this-&gt;callback = false;
return call_user_func($callback, $data);
} else {
return $data;
}
} else {
return curl_error($ch);
}
}

function get($url) {
return $this-&gt;doRequest(‘GET’, $url, ‘NULL’);
}

function post($url, $vars) {
return $this-&gt;doRequest(‘POST’, $url, $vars);
}
}
?&gt;