---
title: '增加VirtualBox虚拟机磁盘容量  '
id: 683
categories:
  - Linux
  - 技术专栏
date: 2013-02-04 15:00:32
tags:
---

默认VirtualBox安装CentOS分配的虚拟磁盘容量为8G，安装完CentOS系统后基本就已经达到4G。实际使用空间很容易就撑爆了。看了网上有一些关于如何扩容的帖子，实际整理的可操作不是很全。这种情况大家都有可能会碰到，我将自己的实际操作记录下来供参考。

1\. 开启CMD命令窗口，进入到VirtualBox安装目录。执行如下命令：

[code lang="js"]
e:\Program Files\Oracle\VirtualBox&gt;VBoxManage.exe modifyhd &quot;F:\Frank4\Frank4.vdi
&quot; --resize 15360
[/code]
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
modifyhd 是命令字，表示扩容； --resize 是参数，参数扩容后的大小（MB）15G； Frank4.vdi  需要扩容的虚拟文件

 <!--more-->

2\. 现在已经增加了VDI的大小，还必须让虚拟机CentOS知道增加的容量，这里需要用到个工具GParted （Gnome Partition Editor）可以在这里http://gparted.sourceforge.net/下载

3\. 在VirtualBox里面，设置CentOS, 启动顺序光驱第一，光驱加载下载的gparted-live ISO

4\. 启动虚拟机CentOS进入GParted,会看到有7GB的容量unallocated,创建Primary Partition，File system: ext4.

应用该操作：

创建后会出现/dev/sda3

5\. 关闭GParted，从VirtualBox设置中卸载虚拟光驱，设置启动顺序为硬盘。

6\. 启动CentOS, 在命令行终端执行扩展逻辑分区

a. 执行 su - root， 按下"Enter"

b. 输入 root用户密码

c. 执行 lvm pvcreate /dev/sda3 ，创建物理卷 /dev/sda3
[code lang="js"]
[root@frank4 frankwu]# &lt;span style=&quot;color:#009900;&quot;&gt;lvm pvcreate /dev/sda3&lt;/span&gt;   Writing physical volume data to disk &quot;/dev/sda3&quot;
  Physical volume &quot;/dev/sda3&quot; successfully created
[root@frank4 frankwu]# df -h -T
Filesystem    Type    Size  Used Avail Use% Mounted on
/dev/mapper/vg_fmaster-lv_root
              ext4    5.5G  4.8G  462M  92% /
tmpfs        tmpfs    499M  112K  499M   1% /dev/shm
/dev/sda1     ext4    485M   76M  384M  17% /boot
[/code]
d. 执行 lvm vgextend "vg_fmaster" /dev/sda3，添加 /dev/sda3到卷组 vg_fmaster
[code lang="js"]
[root@frank4 frankwu]# &lt;span style=&quot;color:#009900;&quot;&gt;lvm vgextend &quot;vg_fmaster&quot; /dev/sda3&lt;/span&gt;   Volume group &quot;vg_fmaster&quot; successfully extended
e. 执行 lvresize -l +100%FREE /dev/mapper/vg_fmaster-lv_root，扩展卷组vg_fmaster下的lv_root卷

[root@frank4 frankwu]# &lt;span style=&quot;color:#009900;&quot;&gt;lvresize -l +100%FREE /dev/mapper/vg_fmaster-lv_root&lt;/span&gt;
  Extending logical volume lv_root to 12.54 GiB
  Logical volume lv_root successfully resized
[/code]
f. 执行 resize2fs /dev/mapper/vg_fmaster-lv_root ，重新设置文件系统
[code lang="js"]
[root@frank4 frankwu]# &lt;span style=&quot;color:#009900;&quot;&gt;resize2fs /dev/mapper/vg_fmaster-lv_root&lt;/span&gt; resize2fs 1.41.12 (17-May-2010)
Filesystem at /dev/mapper/vg_fmaster-lv_root is mounted on /; on-line resizing required
old desc_blocks = 1, new_desc_blocks = 1
Performing an on-line resize of /dev/mapper/vg_fmaster-lv_root to 3286016 (4k) blocks.
The filesystem on /dev/mapper/vg_fmaster-lv_root is now 3286016 blocks long.
[/code]
7\. 验证 df -h -T
[code lang="js"]
[root@frank4 frankwu]# df -h -T
Filesystem    Type    Size  Used Avail Use% Mounted on
/dev/mapper/vg_fmaster-lv_root
              ext4     13G  4.8G  7.0G  41% /
tmpfs        tmpfs    499M  272K  499M   1% /dev/shm
/dev/sda1     ext4    485M   76M  384M  17% /boot
[/code]

非原创，转载自： http://blog.163.com/hnhu_1987/blog/static/2000773042012992426414/