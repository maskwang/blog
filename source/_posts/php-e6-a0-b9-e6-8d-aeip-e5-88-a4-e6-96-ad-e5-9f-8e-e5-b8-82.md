---
title: php根据ip判断城市
id: 117
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:48:43
tags:
---

geiip.php文件
&lt;?php
//返回当前IP的城市字符串
function convertip($ip) {
//IP数据文件路径
$dat_path = 'ip.Dat';

//检查IP地址
if(!preg_match("/^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1
\d\d|2[0-4]\d|25[0-5])\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])$/", $ip)) {
return 'IP Address Error';
}
//打开IP数据文件
if(!$fd = @fopen($dat_path, 'rb')){
return 'IP date file not exists or access denied';
}

//分解IP进行运算，得出整形数
$ip = explode('.', $ip);
$ipNum = $ip[0] * 16777216 + $ip[1] * 65536 + $ip[2] * 256 + $ip[3];

//获取IP数据索引开始和结束位置
$DataBegin = fread($fd, 4);
$DataEnd = fread($fd, 4);
$ipbegin = implode('', unpack('L', $DataBegin));
if($ipbegin &lt; 0) $ipbegin += pow(2, 32);
$ipend = implode('', unpack('L', $DataEnd));
if($ipend &lt; 0) $ipend += pow(2, 32);
$ipAllNum = ($ipend - $ipbegin) / 7 + 1;

$BeginNum = 0;
$EndNum = $ipAllNum;

//使用二分查找法从索引记录中搜索匹配的IP记录
while($ip1num&gt;$ipNum || $ip2num&lt;$ipNum) {
$Middle= intval(($EndNum + $BeginNum) / 2);

//偏移指针到索引位置读取4个字节
fseek($fd, $ipbegin + 7 * $Middle);
$ipData1 = fread($fd, 4);
if(strlen($ipData1) &lt; 4) {
fclose($fd);
return 'System Error';
}
//提取出来的数据转换成长整形，如果数据是负数则加上2的32次幂
$ip1num = implode('', unpack('L', $ipData1));
if($ip1num &lt; 0) $ip1num += pow(2, 32);

//提取的长整型数大于我们IP地址则修改结束位置进行下一次循环
if($ip1num &gt; $ipNum) {
$EndNum = $Middle;
continue;
}

//取完上一个索引后取下一个索引
$DataSeek = fread($fd, 3);
if(strlen($DataSeek) &lt; 3) {
fclose($fd);
return 'System Error';
}
$DataSeek = implode('', unpack('L', $DataSeek.chr(0)));
fseek($fd, $DataSeek);
$ipData2 = fread($fd, 4);
if(strlen($ipData2) &lt; 4) {
fclose($fd);
return 'System Error';
}
$ip2num = implode('', unpack('L', $ipData2));
if($ip2num &lt; 0) $ip2num += pow(2, 32);

//没找到提示未知
if($ip2num &lt; $ipNum) {
if($Middle == $BeginNum) {
fclose($fd);
return 'Unknown';
}
$BeginNum = $Middle;
}
}

$ipFlag = fread($fd, 1);
if($ipFlag == chr(1)) {
$ipSeek = fread($fd, 3);
if(strlen($ipSeek) &lt; 3) {
fclose($fd);
return 'System Error';
}
$ipSeek = implode('', unpack('L', $ipSeek.chr(0)));
fseek($fd, $ipSeek);
$ipFlag = fread($fd, 1);
}

if($ipFlag == chr(2)) {
$AddrSeek = fread($fd, 3);
if(strlen($AddrSeek) &lt; 3) {
fclose($fd);
return 'System Error';
}
$ipFlag = fread($fd, 1);
if($ipFlag == chr(2)) {
$AddrSeek2 = fread($fd, 3);
if(strlen($AddrSeek2) &lt; 3) {
fclose($fd);
return 'System Error';
}
$AddrSeek2 = implode('', unpack('L', $AddrSeek2.chr(0)));
fseek($fd, $AddrSeek2);
} else {
fseek($fd, -1, SEEK_CUR);
}

while(($char = fread($fd, 1)) != chr(0))
$ipAddr2 .= $char;

$AddrSeek = implode('', unpack('L', $AddrSeek.chr(0)));
fseek($fd, $AddrSeek);

while(($char = fread($fd, 1)) != chr(0))
$ipAddr1 .= $char;
} else {
fseek($fd, -1, SEEK_CUR);
while(($char = fread($fd, 1)) != chr(0))
$ipAddr1 .= $char;

$ipFlag = fread($fd, 1);
if($ipFlag == chr(2)) {
$AddrSeek2 = fread($fd, 3);
if(strlen($AddrSeek2) &lt; 3) {
fclose($fd);
return 'System Error';
}
$AddrSeek2 = implode('', unpack('L', $AddrSeek2.chr(0)));
fseek($fd, $AddrSeek2);
} else {
fseek($fd, -1, SEEK_CUR);
}
while(($char = fread($fd, 1)) != chr(0)){
$ipAddr2 .= $char;
}
}
fclose($fd);

//最后做相应的替换操作后返回结果
if(preg_match('/http/i', $ipAddr2)) {
$ipAddr2 = '';
}
$ipaddr = "$ipAddr1 $ipAddr2";
$ipaddr = preg_replace('/CZ88.Net/is', '', $ipaddr);
$ipaddr = preg_replace('/^s*/is', '', $ipaddr);
$ipaddr = preg_replace('/s*$/is', '', $ipaddr);
if(preg_match('/http/i', $ipaddr) || $ipaddr == '') {
$ipaddr = 'Unknown';
}

return $ipaddr;
}
//查找字符串
function findstr($str, $substr)
{
$m = strlen($str);
$n = strlen($substr);
if ($m &lt; $n) return false ;
for ($i=0; $i &lt;=($m-$n+1); $i ++){
$sub = substr( $str, $i, $n);
if ( strcmp($sub, $substr) == 0) return true;
}
return false ;
}
?&gt;

getcity.php文件
&lt;?php
include("getip.php");
//$Clientip=$_SERVER["REMOTE_ADDR"];
$Clientip="59.172.58.74";
$ClientSity=convertip($Clientip);      //echo $ClientSity=convertip($Clientip);
$cityarr = array("北京","上海","武汉","广州");
foreach ($cityarr as $city) {
if (findstr($ClientSity,$city)){
echo "来自".$city;
$getcity = 1;
break;
}
}

if (1 != $getcity) {
echo "来自全国";
}
?&gt;

<span style="color: #ff0000;">**<span style="font-size: large;">PHPCHINA解决方案：</span>**</span>

两种方式
1.调用腾讯的API接口。在浏览器中输入以下链接：http://   fw.qq.com/ipaddress，会返回你的一些相关信息。以我自己为例，得到的结果是：var IPData = new Array("219.136.136.220","","广东省","广州市");
然后再对这个返回结果做以下处理，步骤如下：
&lt;?php
function get_ip_place(){
$ip=file_get_contents("http://    fw.qq.com/ipaddress");
$ip=str_replace('"',' ',$ip);
$ip2=explode("(",$ip);
$a=substr($ip2[1],0,-2);
$b=explode(",",$a);
return $b;
}
$ip=get_ip_place();
print_r($ip);
print_r($ip[3]);  //这个就是你需要的城市了
?&gt;
*注意把http://    fw.qq.com/ipaddress中的空格去掉。因为我没有权限发URL链接，所以只能这样写了。

2.在网上找到IP对应城市的库文件，把这些记录添加到数据库里。
用户访问的时候获取用户的IP,再通过数据库表查询。