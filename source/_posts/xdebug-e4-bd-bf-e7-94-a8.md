---
title: 'xdebug 使用  '
id: 461
categories:
  - Php
  - 技术专栏
  - 编程语言
date: 2011-12-14 11:39:51
tags:
---

**一、安装xdebug模块**
1、去www.xdebug.org下载相应版本php的模块文件，保存下载后的文件到php的ext目录，可以自己修改文件的名称，现在最新的版本是 2.0.1。
2、修改php.ini，增加如下信息
1.[Xdebug]
2.zend_extension_ts="d:/php/ext/xdebug-xxx.dll"
3.xdebug.auto_trace=on
4.xdebug.collect_params=on
5.xdebug.collect_return=on
6.xdebug.trace_output_dir="d:\Temp\xdebug"
7.xdebug.profiler_enable=on
8.xdebug.profiler_output_dir="d:\Temp\xdebug"

参数解释：
zend_extension_ts="d:/php/ext/xdebug-xxx.dll"
加 载xdebug模块。这里不能用extension=xdebug-xxx.dll的方式加载，必须要以zend的方式加载，否则安装上后， phpinfo打印出来的里的xdebug段的会有XDEBUG NOT LOADED AS ZEND EXTENSION的警告信息。

xdebug.auto_trace=on
自动打开“监测函数调用过程”的功模。该功能可以在你指定的目录中将函数调用的监测信息以文件的形式输出。此配置项的默认值为off。

xdebug.collect_params=on
打开收集“函数参数”的功能。将函数调用的参数值列入函数过程调用的监测信息中。此配置项的默认值为off。

xdebug.collect_return=on
打开收集“函数返回值”的功能。将函数的返回值列入函数过程调用的监测信息中。此配置项的默认值为off。

xdebug.trace_output_dir="d:\Temp\xdebug"
设定函数调用监测信息的输出文件的路径。

xdebug.profiler_enable=on
打开效能监测器。

xdebug.profiler_output_dir="d:\Temp\xdebug"
设定效能监测信息输出文件的路径。

另外，xdebug 不能和 Zend Optimizer 以及其他 Zend 扩展 (DBG, APC, APD etc) 同时工作，目前这个问题正在修复中。

还有一些更为具体的参数设定，详见：http://www.xdebug.org/docs-settings.php

3、重启apache

这样，在本地运行php的时候，会在所设定的目录里产生一些调试信息的文件：

* 函数调用过程监测信息文件的文件名格式：trace.××××××.xt。这个文件可以直接查看，里面包含了函数运行的时间，函数调用的参数值，返回值，所在的文件和位置等信息。内容格式还是相对直观的。
* 效能监测文件的文件名格式：cachegrind.out.××××××××。
这个文件也可以直接查看，不过信息格式不易被人类所理解，
所以我们需要接下来的一个软件。

二、安装wincachegrind
由于效能监测文件：cachegrind.out.××××××××文件的内容不易被人类所理解，所以我们需要一个工具来读取它。windows下就有一款这样的软件：wincachegrind。
1、到http://sourceforge.net/projects/wincachegrind/下载安装wincachegrind
2、安装运行后，点击Tools-&gt;options，设定你的working folder(php.ini里xdebug.profiler_output_dir的值)
这样就可以比较直观的查看效能监测文件的信息了。

控制输出CacheGrind文件名的控制
http://xdebug.org/docs/all_settings#trace_output_name

如何利用Xdebug测试脚本执行时间
测试某段脚本的执行时间，通常我们都需要用到microtime()函数来确定当前时间。例如PHP手册上的例子：
&lt;?php
/**
* Simple function to replicate PHP 5 behaviour
*/
function microtime_float()
{
list($usec, $sec) = explode(" ", microtime());
return ((float)$usec + (float)$sec);
}

$time_start = microtime_float();
// Sleep for a while
usleep(100);
$time_end = microtime_float();
$time = $time_end - $time_start;
echo "Did nothing in $time seconds\n";
?&gt;
但是microtime()返回的值是微秒数及绝对时间戳（例如“0.03520000 1153122275”），没有可读性。所以如上程序，我们需要另外写一个函数microtime_float()，来将两者相加。

Xdebug自带了一个函数xdebug_time_index()来显示时间。

&nbsp;

**如何测定脚本占用的内存？**

&nbsp;

有时候我们想知道程序执行到某个特定阶段时到底占用了多大内存，为此PHP提供了函数memory_get_usage()。这个函数只有当PHP编译时使用了--enable-memory-limit参数时才有效。

Xdebug同样提供了一个函数xdebug_memory_usage()来实现这样的功能，另外xdebug还提供了一个xdebug_peak_memory_usage()函数来查看内存占用的峰值。

&nbsp;

**如何检测代码中的不足？**

有 时候代码没有明显的编写错误，没有显示任何错误信息（如error、warning、notice等），但是这不表明代码就是正确无误的。有时候可能某段 代码执行时间过长，占用内存过多以致于影响整个系统的效率，我们没有办法直接看出来是哪部份代码出了问题。这时候我们希望把代码的每个阶段的运行情况都监 控起来，写到日志文件中去，运行一段时间后再进行分析，找到问题所在。

相关参数设置
xdebug.default_enable
类型：布尔型 默认值：On
如果这项设置为On，堆栈跟踪将被默认的显示在错误事件中。你可以通过在代码中使用xdebug_disable()来禁止堆叠跟踪的显示。因为这是xdebug基本功能之一，将这项参数设置为On是比较明智的。

xdebug.max_nesting_level
类型：整型 默认值：100
The value of this setting is the maximum level of nested functions that are allowed before the script will be aborted.
限制无限递归的访问深度。这项参数设置的值是脚本失败前所允许的嵌套程序的最大访问深度。

第三部分：堆栈跟踪:
相关参数设置
xdebug.dump_globals
类型：布尔型 默认值：1
限制是否显示被xdebug.dump.*设置定义的超全局变量的值
例 如，xdebug.dump.SERVER = REQUEST_METHOD,REQUEST_URI,HTTP_USER_AGENT 将打印 PHP 超全局变量 $_SERVER['REQUEST_METHOD']、$_SERVER['REQUEST_URI'] 和 $_SERVER['HTTP_USER_AGENT']。

xdebug.dump_on
<div><wbr>ce
类型：布尔型 默认值：1
限制是否超全局变量的值应该转储在所有出错环境(设置为Off时)或仅仅在开始的地方(设置为On时)

xdebug.dump_undefined
类型：布尔型 默认值：0
如果你想从超全局变量中转储未定义的值，你应该把这个参数设置成On，否则就设置成Off

xdebug.show_exception_trace
类型：整型 默认值：0
当这个参数被设置为1时，即使捕捉到异常，xdebug仍将强制执行异常跟踪当一个异常出现时。

xdebug.show_local_vars
类型：整型 默认值：0
当这个参数被设置为不等于0时，xdebug在错环境中所产生的堆栈转储还将显示所有局部变量，包括尚未初始化的变量在最上面。要注意的是这将产生大量的信息，也因此默认情况下是关闭的。

第四部分：分析PHP脚本
相关参数设置
xdebug.profiler_append
类型：整型 默认值：0
当这个参数被设置为1时，文件将不会被追加当一个新的需求到一个相同的文件时(依靠xdebug.profiler_output_name的设置)。相反的设置的话，文件将被附加成一个新文件。

xdebug.profiler_enable
类型：整型 默认值：0
开放xdebug文件的权限，就是在文件输出目录中创建文件。那些文件可以通过KCacheGrind来阅读来展现你的数据。这个设置不能通过在你的脚本中调用ini_set()来设置。

xdebug.profiler_output_dir
类型：字符串 默认值：/tmp
这个文件是profiler文件输出写入的，确信PHP用户对这个目录有写入的权限。这个设置不能通过在你的脚本中调用ini_set()来设置。

xdebug.profiler_output_name
类型：字符串 默认值：cachegrind.out%p
这个设置决定了转储跟踪写入的文件的名称。

第五部分：远程Debug
相关参数设置
xdebug.remote_autostart
类型：布尔型 默认值：0
一般来说，你需要使用明确的HTTP GET/POST变量来开启远程debug。而当这个参数设置为On，xdebug将经常试图去开启一个远程debug session并试图去连接客户端，即使GET/POST/COOKIE变量不是当前的。

xdebug.remote_enable
类型：布尔型 默认值：0
这个开关控制xdebug是否应该试着去连接一个按照xdebug.remote_host和xdebug.remote_port来设置监听主机和端口的debug客户端。

xdebug.remote_host
类型：字符串 默认值：localhost
选择debug客户端正在运行的主机，你不仅可以使用主机名还可以使用IP地址

xdebug.remote_port
类型：整型 默认值：9000
这个端口是xdebug试着去连接远程主机的。9000是一般客户端和被绑定的debug客户端默认的端口。许多客户端都使用这个端口数字，最好不要去修改这个设置。</wbr></div>