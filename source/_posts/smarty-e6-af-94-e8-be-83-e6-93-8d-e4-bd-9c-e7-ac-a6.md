---
title: smarty 比较操作符
id: 56
categories:
  - Smarty
  - 技术专栏
date: 2011-08-22 22:31:45
tags:
---

<div id="blog_text">
<div>

eq相等，
f7Y"Y f3m @,V+Z }4X0 ne、neq不相等，
7C E z R S8P t,i:@ ^0 gt大于，
4d D H4F,Yg;D X)Y0 lt小于，PHPChina 开源社区门户 T f D |(s P9e
gte、ge大于等于，PHPChina 开源社区门户.L | \8i-C n:Q7]
lte、le 小于等于，PHPChina 开源社区门户 L8^ S \;u5_ ^$n/i"D
not非， mod求模。 PHPChina 开源社区门户 i ^ p a S&amp;a
is [not] div by是否能被某数整除，
,B z M E m I w0 is [not] even是否为偶数，PHPChina 开源社区门户;J K H t"g o { I
$a is [not] even by $b即($a / $b) % 2 == 0，PHPChina 开源社区门户#K g P e!?/k$T e k e3F
is [not] odd是否为奇，PHPChina 开源社区门户 c b1^ F F e p7L
$a is not odd by $b即($a / $b) % 2 != 0 示例：

equal/ not equal/ greater than/ less than/ less than or equal/ great than or equal/后面的就不用说了

Smarty 中的 if 语句和 php 中的 if 语句一样灵活易用，并增加了几个特性以适宜模板引擎. if 必须于 /if 成对出现. 可以使用 else 和 elseif 子句. 可以使用以下条件修饰词：eq、ne、neq、gt、lt、lte、le、gte、ge、is even、is odd、is not even、is not odd、not、mod、div by、even by、odd by、==、!=、&gt;、&lt;、&lt;=、&gt;=. 使用这些修饰词时必须和变量或常量用空格格开.

Example 7-11\. if statements
例 7-11\. if 语句演示

{if $name eq "Fred"}
Welcome Sir.
{elseif $name eq "Wilma"}
Welcome Ma'am.
{else}
Welcome, whatever you are.
{/if}

{* an example with "or" logic *}
{if $name eq "Fred" or $name eq "Wilma"}
...
{/if}

{* same as above *}
{if $name == "Fred" || $name == "Wilma"}
...
{/if}

{* the following syntax will NOT work, conditional qualifiers
must be separated from surrounding elements by spaces *}
{if $name=="Fred" || $name=="Wilma"}
...
{/if}

{* parenthesis are allowed *}
{if ( $amount &lt; 0 or $amount &gt; 1000 ) and $volume &gt;= #minVolAmt#}
...
{/if}

{* you can also embed php function calls *}
{if count($var) gt 0}
...
{/if}

{* test if values are even or odd *}
{if $var is even}
...
{/if}
{if $var is odd}
...
{/if}
{if $var is not odd}
...
{/if}

{* test if var is divisible by 4 *}
{if $var is div by 4}
...
{/if}

{* test if var is even, grouped by two. i.e.,
0=even, 1=even, 2=odd, 3=odd, 4=even, 5=even, etc. *}
{if $var is even by 2}
...
{/if}

{* 0=even, 1=even, 2=even, 3=odd, 4=odd, 5=odd, etc. *}
{if $var is even by 3}
...
{/if}

</div>
</div>