---
title: SCP 命令详解
id: 443
categories:
  - Linux
  - 技术专栏
date: 2011-11-02 14:50:17
tags:
---

SCP 命令跟 CP 命令类似，只不过CP命令是在同一台机器上用的，SCP是在2台机器上复制的命令。

举例说明：
复制远程服务器的文件到本地
<div>
<div>scp -P 999 root@21andy.com:/home/test.zip /home/</div>
</div>
复制本地的目录到远程服务器
<div>
<div>scp -P 999 -r /home/andy/ root@21andy.com:/home/</div>
</div>
**SCP 命令语法**
scp [-1245BCpqrv] [-c cipher] [F ssh_config] [-I identity_file] [-l limit] [-o ssh_option] [-P port] [-S program] [[user@]host1:] file1 […] [[suer@]host2:]file2

**SCP 命令说明**
Scp在主机间复制文件。他使用 ssh(1)作为数据传输。而且用同样认证和安全性。 scp将在认证中请求输入密码所有的文件可能需要服务器和用户的特别描述来指明文件将被复制到/从某台服务器。两个远程登录的服务器间的文件复制是允许的。

**SCP 命令选项**
-1 强制scp 用协议1
-2 强制scp 用协议2
-4 强制scp用IPV4的网址
-6 强制scp用IPV6的网址
-B 选择批处理模式（防止输入密码）
-C 允许压缩。 标注-C到ssh(1)来允许压缩
-c cipher
选择cipher来加密数据传输。这个选项直接传递到ssh(1)
-F ssh_config
设定一个可变动的用户配置给ssh.这个选项直接会被传递到ssh(1)
-i identity_file
选择被RSA认证读取私有密码的文件。这个选项可以直接被传递到ssh(1)
-l limit
限制传输带宽，也就是速度 用Kbit/s的速度
-o ssh_option
可以把ssh_config中的配置格式传到ssh中。这种模式对于说明没有独立的scp文件中断符的scp很有帮助。关于选项的如下。而他们的值请参看ssh_config(5)
-P port
指定连接远程连接端口。注意这个选项需要写成大写的模式。因为-p已经早保留了次数和模式
-S program
指定一个加密程序。这个程序必须可读所有ssh(1)的选项。
-p 指定修改次数，连接次数，还有对于原文件的模式
-q 把进度参数关掉
-r 递归的复制整个文件夹
-S program
指定一个加密程序。这个程序必须可读所有ssh(1)的选项。
-V 冗余模式。 让 scp 和 ssh(1) 打印他们的排错信息， 这个在排错连接，认证，和配置中非常有用。

**SCP 命令诊断**
scp 返回0 成功时，不成功时返回值大于0

**SCP 命令不需要输入用户密码的使用方法**

在两台机器的两个用户之间**建立安全的信任关系**后，可实现执行scp命令时不需要输入用户密码。

1\. 在机器A上root用户执行 **ssh-keygen** 命令，生成建立安全信任关系的证书。
<div>
<div>[root@A root]# ssh-keygen -b 1024 -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): &lt;– 直接输入回车
Enter passphrase (empty for no passphrase): &lt;– 直接输入回车
Enter same passphrase again: &lt;– 直接输入回车
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is: ……</div>
</div>
注意：在程序提示输入 passphrase 时直接输入回车，表示无证书密码。
上述命令将生成私钥证书 **id_rsa **和公钥证书 **id_rsa.pub**，存放在用户目录的 **.ssh** 子目录中。

2\. 将公钥证书 **id_rsa.pub **复制到机器B的root目录的.ssh子目录中，同时将文件名更换为**authorized_keys**。
<div>
<div>[root@A root]# scp -p .ssh/id_rsa.pub root@机器B的IP:/root/.ssh/authorized_keys
root@192.168.3.206’s password: &lt;– 输入机器B的root用户密码</div>
</div>
在执行上述命令时，两台机器的root用户之间还未建立安全信任关系，所以还需要输入机器B的root用户密码。
经过以上2步，就在机器A的root和机器B的root之间建立安全信任关系。下面我们看看效果：
<div>
<div>[root@A root]# scp -p test root@机器B的IP地址:/root</div>
</div>
成功了！真的不再需要输入密码了。