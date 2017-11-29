---
title: Centos上用yum命令进行更新
id: 377
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:57:24
tags:
---

最近接触Centos，这个系统相当于RedHat Enterprise的免费版，很好很强大。
刚刚接触，所以记录一下在centos上做更新的方法，我用的是centos5。
yum是一个很好的管理rpm包的程序，yum客户端可以通过http、ftp方式获得软件包，并使用方便的命令直接管理、更新所有的rpm包，甚至包括kernel的更新。

#yum check-update   检查可更新的rpm包
#yum update   更新所有的rpm包
#yum update 包名 更新指定的rpm包

要进行更新的时候，首先找到一个最近比较稳定，速度比较快的更新源，我找到的是这个源：http://mirror.be10.com/centos /RPM-GPG-KEY-CentOS-5。然后将/etc/yum.repos.d/CentOS-Base.repo文件移走，从新建立一个 CentOS-Base.repo，在文件里输入：

[base]
name=CentOS-$releasever - Base
baseurl=http://mirror.be10.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

#released updates
[update]
name=CentOS-$releasever - Updates
baseurl=http://mirror.be10.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

#packages used/produced in the build but not released
[addons]
name=CentOS-$releasever - Addons
baseurl=http://mirror.be10.com/centos/$releasever/addons/$basearch/
gpgcheck=1
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=http://mirror.be10.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=http://mirror.be10.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
baseurl=http://mirror.be10.com/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirror.be10.com/centos/RPM-GPG-KEY-CentOS-5

导入官方的Key
#rpm --import http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-5
然后开始更新
#yum upgrade
当yum资源库有更新时，yum会自动下载所有所需的headers放置于/var/cache/yum目录下，然后再问你是否更新这些资源，此时输入“y”回车就开始下载包资源了。不要在后台运行yum upgrade，否则输入y后没有响应。