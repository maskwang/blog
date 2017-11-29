---
title: 'shell 编程之2>&1 '
id: 451
categories:
  - Linux
  - 技术专栏
date: 2011-11-17 13:43:57
tags:
---

<div id="app-share-content">
<div>

经常可以在一些脚本，尤其是在crontab调用时发现如下形式的命令调用
<div>
<div>/tmp/test.sh &gt; /tmp/test.log 2&gt;&amp;1</div>
</div>
前半部分/tmp/test.sh &gt; /tmp/test.log很容易理解，那么后面的2&gt;&amp;1是怎么回事呢？

要解释这个问题，还是得提到文件重定向。我们知道&gt;和&lt;是文件重定向符。那么1和2是什么？在shell中，每个进程都和三个系统文件 相关联：标准输入stdin，标准输出stdout和标准错误stderr，三个系统文件的文件描述符分别为0，1和2。所以这里2&gt;&amp;1 的意思就是将标准错误也输出到标准输出当中。

下面通过一个例子来展示2&gt;&amp;1有什么作用：
<div>
<div>$ cat test.sh
t
date</div>
</div>
test.sh中包含两个命令，其中t是一个不存在的命令，执行会报错，默认情况下，错误会输出到stderr。date则能正确执行，并且输出时间信息，默认输出到stdout
<div>
<div>./test.sh &gt; test1.log
./test.sh: line 1: t: command not found

$ cat test1.log
Tue Oct 9 20:51:50 CST 2007</div>
</div>
可以看到，date的执行结果被重定向到log文件中了，而t无法执行的错误则只打印在屏幕上。
<div>
<div>$ ./test.sh &gt; test2.log 2&gt;&amp;1

$ cat test2.log
./test.sh: line 1: t: command not found
Tue Oct 9 20:53:44 CST 2007</div>
</div>
这次，stderr和stdout的内容都被重定向到log文件中了。

实际上， **&gt;** 就相当于 **1&gt;** 也就是重定向标准输出，不包括标准错误。通过2&gt;&amp;1，就将标准错误重定向到标准输出了，那么再使用&gt;重定向就会将标准输出和标准错误信息一同重定向了。如果只想重定向标准错误到文件中，则可以使用**2&gt; file**。

</div>
</div>