---
title: php多维数组排序，usort函数
id: 68
categories:
  - Php
  - 技术专栏
date: 2011-08-22 22:35:45
tags:
---

<div id="blog_text">private function getUserByIds(){
function cmp($a, $b) {
if ($a['visits'] &lt; $b['visits']) {
return true;
}
else {
return false;
}
}

echo "正在执行多条ID查询&lt;hr/&gt;";
$sort = (isset($this-&gt;_get['sort'])) ? $this-&gt;_get['sort'] : '';
$userid = (isset($this-&gt;_get['userid'])) ? $this-&gt;_get['userid'] : '';
$userid=explode(',',$userid);
$userModule = new ZoneUserModule();
$users = array();
try {

for ($i=0;$i&lt;count($userid);$i++){
//echo $userid[$i]."&lt;br/&gt;";
$userdata[$i] = $userModule-&gt;getUserId($userid[$i]);
//print_r($userdata[$i]);
}
usort($userdata, 'cmp');
foreach ($userdata as $value) {
//var_dump($value['visits']);
echo '&lt;br /&gt;&lt;br /&gt;';
}
//echo count($userdata)."&lt;hr/&gt;";
//echo $userdata[0][visits];
//print_r($userdata);
if($sort == 'visitAsc'){
$userdata = array_reverse($userdata);//反向输出数组array_reverse（）
}</div>