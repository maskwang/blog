---
title: php curl 支持 https
id: 493
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2012-02-10 12:30:15
tags:
---

PHP co<wbr>de

<pre lang="php">
<?php
$url='https://login.yahoo.com/config/login?';
//open connection
$ch = curl_init();

//set the url, number of POST vars, POST da<wbr>ta
curl_setopt($ch,CURLOPT_URL,$url);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);// allow redirects
curl_setopt($ch, CURLOPT_RETURNTRANSFER,1); // return into a variable

//主要这两句
## Below two option will enable the HTTPS option.
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);

curl_setopt($ch, CURLOPT_SSL_VERIFYHOST,  FALSE);

//execute post
$result = curl_exec($ch);
//close connection
curl_close($ch);

echo $result;
?>
</pre>