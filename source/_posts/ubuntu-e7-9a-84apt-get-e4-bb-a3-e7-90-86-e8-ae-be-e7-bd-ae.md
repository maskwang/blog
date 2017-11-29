---
title: Ubuntu的apt-get代理设置
id: 379
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:57:53
tags:
---

<div id="blog_text">
<div>

升级到Ubuntu10.04后，发现apt-get的代理设置有改变了，在9.10以前使用“http_proxy”环境变量就可以令apt-get使用代理了
<div>
<div>
<pre>export http_proxy=http://127.0.0.1:8000
sudo apt-get update</pre>
</div>
</div>
然后在Ubuntu10.04下就无效了，看来apt-get已经被改成不使用这个环境变量了。

一阵郁闷后，最后我发现在“首选项”-&gt;“网络代理”那里，多了个“System-wide”按钮（我用的是英文环境，不知道中文被翻译成怎样，关闭窗口时也会提示你），在这里设置后，apt-get确实可以使用代理了。

但是我依然鄙视这种改进，因为我通常就是偶尔使用代理，更新几个被墙掉的仓库而已（如dropbox和tor），根本不想使用全局代理，本来用终端就能搞定的事，现在切换代理要点N次鼠标，真烦。

所以我研究了一下，发现那个代理设置修改了两个文件，一个是“/etc/environment”，这个是系统的环境变量，里面定义了“http_proxy”等代理环境变量。另一个是“/etc/apt/apt.conf”，这个就是apt的配置，内容如下
<div>
<div>
<pre>Acquire::http::proxy "http://127.0.0.1:8000/";
Acquire::ftp::proxy "ftp://127.0.0.1:8000/";
Acquire::https::proxy "https://127.0.0.1:8000/";</pre>
</div>
</div>
很明显的代理设置代码，我看了下apt-get的手册，发现可以用“-c”选项来指定使用配置文件，也就是复制一份为“~/apt_proxy.conf”，然后“网络代理”那里重置回直接连接，以后使用
<div>
<div>
<pre>sudo apt-get -c ~/apt_proxy.conf update</pre>
</div>
</div>
就可以使用代理了，apt-get也有一个“-o”选项，直接跟apt-get的设置变量，就不用指定配置文件了，比如
<div>
<div>
<pre>sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:8000/" update</pre>
</div>
</div>
</div>
</div>