---
title: configure/make/make install的作用
id: 359
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:53:10
tags:
---

这些都是典型的使用GNU的AUTOCONF和AUTOMAKE产生的程序的安装步骤。

**./configure** 是用来检测你的安装平台的目标特征的。比如它会检测你是不是有CC或GCC，并不是需要CC或GCC，它是个shell脚本。

**make** 是用来编译的，它从Makefile中读取指令，然后编译。

**make install** 是用来安装的，它也从Makefile中读取指令，安装到指定的位置。

**AUTOMAKE和AUTOCONF** 是非常有用的用来发布C程序的东西。如果你也写程序想使用AUTOMAKE和AUTOCONF，可以参考CNGNU.ORG上的相关文章。

--End--

**1、configure**，这一步一般用来生成 Makefile，为下一步的编译做准备，你可以通过在 configure 后加上参数来对安装进行控制，比如
<div>代码:</div>
<div>./configure --prefix=/usr
上面的意思是将该软件安装在 /usr 下面，执行文件就会安装在 /usr/bin （而不是默认的 /usr/local/bin),资源文件就会安装在 /usr/share（而不是默认的/usr/local/share）。同时一些软件的配置文件你可以通过指定 --sys-config= 参数进行设定。有一些软件还可以加上 --with、--enable、--without、--disable 等等参数对编译加以控制，你可以通过允许 ./configure --help 察看详细的说明帮助。

**    2、make**， 这一步就是编译，大多数的源代码包都经过这一步进行编译（当然有些perl或python编写的软件需要调用perl或python来进行编译）。如果在 make 过程中出现 error ，你就要记下错误代码（注意不仅仅是最后一行），然后你可以向开发者提交 bugreport（一般在 INSTALL 里有提交地址），或者你的系统少了一些依赖库等，这些需要自己仔细研究错误代码。

**    3、make insatll**，这条命令来进行安装（当然有些软件需要先运行 make check 或 make test 来进行一些测试），这一步一般需要你有 root 权限（因为要向系统写入文件）。</div>