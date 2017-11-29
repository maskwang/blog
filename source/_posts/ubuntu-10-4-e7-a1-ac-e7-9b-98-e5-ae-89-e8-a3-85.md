---
title: ubuntu 10.4 硬盘安装
id: 88
categories:
  - Linux
  - 技术专栏
date: 2011-08-22 22:40:31
tags:
---

硬盘安装ubuntu10.04 安装linux次数比较多了，但是每次安装都要从头看起。所以，这次安装之后，我决定把自己的安装过程写下来，以备后用。 ubuntu10.04，我一直在等beta版出来，试试在我本本上的安装效果。结果发现，驱动支持堪称完美，连无线网卡驱动都不用装。对于这款2010 年元月份才上市的i3本来说实属不易，就冲这点儿也要谢谢ubuntu的开发团队。 言归正传，对于我这种不爱刻录的人（其实以前是没有刻录机，硬盘安装惯了），硬盘安装变成了最后的选择了。首先，我会在xp下用PM分区工具新建两个分 区，一个当作交换分区，一个作为ubuntu根目录挂载分区。交换分区当然要选择linux swap格式，而挂载分区就无所谓了，因为在安装的时候格式化为ext4也不迟。 第二、将下载好的ubuntu10.04安装文件放在任何格式的分区的根目录下（当然不能在你要安装的分区下面），比如说是E盘根目录下，然后用解压工具 把casper文件夹下面的vmlinuz和initrd.lz文件也解压到E盘根目录。在E盘根目录下新建一个文本文件命名为menu.lst，里面写 上 title Install Ubuntu find --set-root /ubuntu-10.04-beta1-desktop-i386.iso kernel /vmlinuz boot=casper iso-scan/filename=/ubuntu-10.04-beta1-desktop-i386.iso locale=zh_CN.UTF-8 initrd /initrd.lz 注意： 这个命令里面的 ubuntu-10.04-beta1-desktop-i386.iso 是你下载的iso文件的名字，你下载的光盘镜像可能不是这个名字，所以要把它改为你自己的文件的名字。 除了这个我们建立的menu.lst的文件 之外，其它盘的根目录下如果也存在着这样的名字的文件，必须全部删除或者重命名。 第三、从网上下载最新版的grub4dos，从中提取出一个名为grldr的文件（只要这一个就够了），把它放到XP系统盘的根目录下。然后修改 boot.ini，在最末加上一句： c:\grldr=”ubuntu” boot.ini是系统隐藏的只读文件，要想修改它，必须先把其属性里的只读去掉。 第四、重启计算机，选择ubuntu。进入安装界面。为了硬盘中的安装镜像不影响整个安装过程，所以先在终端里执行下面的命令：sudo umount -l /isodevice 第五、如果在上网状态，把网线先拔掉（避免79%进度时 配置apt时间过长而不往下面进行）。点击安装ubuntu。开始很简单，到预备硬盘空间时，选“手动指定分区”，跟据你自己情况选择交换空间和根目录挂 载分区。默认状态，把grub写入mbr，这可以不用管。 第六、95%进度 配置kdpk时，需要耐心等待，终究会安装完成的。安装完毕后，重启自动进入ubuntu10.04。可以进安装文件所在的那个分区，删除vmlinuz 和initrd.lz, 删除menu.lst. 第七、更新源，配置grub2。 更详细的图文安装教程，请参考http://forum.ubuntu.org.cn/viewtopic.php?f=77&amp; t=217161&amp;start=0