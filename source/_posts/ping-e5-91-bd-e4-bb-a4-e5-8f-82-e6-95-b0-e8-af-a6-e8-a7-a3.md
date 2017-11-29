---
title: PING命令参数详解
id: 173
categories:
  - 其它
  - 技术专栏
date: 2011-08-22 23:13:09
tags:
---

<div id="blog_text">

<span style="color: #0000ff;">[编辑本段]Ping概述：

何为ping？

是DOS命令，一般用于检测网络通与不通 ，也叫时延，其值越大，速度越慢
PING (Packet Internet Grope)，因特网包探索器，用于测试网络连接量的程序。Ping</span>

<span style="color: #0000ff;">发送一个ICMP回声请求消息给目的地并报告是否收到所希望的ICMP回声应答。
它是用来检查网络是否通畅或者网络连接速度的命令。作为一个生活在网络上的管理员</span>

<span style="color: #0000ff;">或者黑客来说，ping命令是第一个必须掌握的DOS命令，它所利用的原理是这样的：网络上</span>

<span style="color: #0000ff;">的机器都有唯一确定的IP地址，我们给目标IP地址发送一个数据包，对方就要返回一个同样</span>

<span style="color: #0000ff;">大小的数据包，根据返回的数据包我们可以确定目标主机的存在，可以初步判断目标主机的</span>

<span style="color: #0000ff;">操作系统等。
Ping 是Windows系列自带的一个可执行命令。利用它可以检查网络是否能够连通，用好</span>

<span style="color: #0000ff;">它可以很好地帮助我们分析判定网络故障。应用格式：Ping IP地址。该命令还可以加许多</span>

<span style="color: #0000ff;">参数使用，具体是键入Ping按回车即可看到详细说明。
ping指的是端对端连通，通常用来作为可用性的检查，
但是某些病毒木马会强行大量远程执行ping命令抢占你的网络资源，导致系统变慢，网</span>

<span style="color: #0000ff;">速变慢。
严禁ping入侵作为大多数防火墙的一个基本功能提供给用户进行选择。
[编辑本段]PING命令参数详解
1、-a 解析计算机NetBios名。
[1]示例：
C:＼&gt;ping -a 192.168.1.21
Pinging iceblood.yofor.com [192.168.1.21] with 32 bytes of data:
Reply from 192.168.1.21: bytes=32 time&lt;10ms TTL=254
Reply from 192.168.1.21: bytes=32 time&lt;10ms TTL=254
Reply from 192.168.1.21: bytes=32 time&lt;10ms TTL=254
Reply from 192.168.1.21: bytes=32 time&lt;10ms TTL=254
Ping statistics for 192.168.1.21:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),Approximate round trip </span>

<span style="color: #0000ff;">times in milli-seconds:
Minimum = 0ms, Maximum = 0ms, Average = 0ms
从上面就可以知道IP为192.168.1.21的计算机NetBios名为iceblood.yofor.com。
2、n count 发送count指定的Echo数据包数。
在默认情况下，一般都只发送四个数据包，通过这个命令可以自己定义发送的个数，</span>

<span style="color: #0000ff;">对衡量网络速度很有帮助，比如我想测试发送50个数据包的返回的平均时间为多少，最快时</span>

<span style="color: #0000ff;">间为多少，最慢时间为多少就可以通过以下获知：
C:＼&gt;ping -n 50 202.103.96.68
Pinging 202.103.96.68 with 32 bytes of data:
Reply from 202.103.96.68: bytes=32 time=50ms TTL=241
Reply from 202.103.96.68: bytes=32 time=50ms TTL=241
Reply from 202.103.96.68: bytes=32 time=50ms TTL=241
Request timed out.
………………
Reply from 202.103.96.68: bytes=32 time=50ms TTL=241
Reply from 202.103.96.68: bytes=32 time=50ms TTL=241
Ping statistics for 202.103.96.68:
Packets: Sent = 50, Received = 48, Lost = 2 (4% loss),Approximate round trip </span>

<span style="color: #0000ff;">times in milli-seconds:
Minimum = 40ms, Maximum = 51ms, Average = 46ms
从以上我就可以知道在给202.103.96.68发送50个数据包的过程当中，返回了48个，其</span>

<span style="color: #0000ff;">中有两个由于未知原因丢失，这48个数据包当中返回速度最快为40ms，最慢为51ms，平均速</span>

<span style="color: #0000ff;">度为46ms。
3、-l size . 定义echo数据包大小。
在默认的情况下windows的ping发送的数据包大小为32byt，我们也可以自己定义它的大</span>

<span style="color: #0000ff;">小，但有一个大小的限制，就是最大只能发送65500byt，也许有人会问为什么要限制到</span>

<span style="color: #0000ff;">65500byt，因为Windows系列的系统都有一个安全漏洞（也许还包括其他系统）就是当向对</span>

<span style="color: #0000ff;">方一次发送的数据包大于或等于65532时，对方就很有可能当机，所以微软公司为了解决这</span>

<span style="color: #0000ff;">一安全漏洞于是限制了ping的数据包大小。虽然微软公司已经做了此限制，但这个参数配合</span>

<span style="color: #0000ff;">其他参数以后危害依然非常强大，比如我们就可以通过配合-t参数来实现一个带有攻击性的</span>

<span style="color: #0000ff;">命令：（以下介绍带有危险性，仅用于试验，请勿轻易施于别人机器上，否则后果自负）
C:＼&gt;ping -l 65500 -t 192.168.1.21
Pinging 192.168.1.21 with 65500 bytes of data:
Reply from 192.168.1.21: bytes=65500 time&lt;10ms TTL=254
Reply from 192.168.1.21: bytes=65500 time&lt;10ms TTL=254
………………
这样它就会不停的向192.168.1.21计算机发送大小为65500byt的数据包，如果你只有一</span>

<span style="color: #0000ff;">台计算机也许没有什么效果，但如果有很多计算机那么就可以使对方完全瘫痪，曾做过这样</span>

<span style="color: #0000ff;">的试验，当同时使用10台以上计算机ping一台Win2000Pro系统的计算机时，不到5分钟对方</span>

<span style="color: #0000ff;">的网络就已经完全瘫痪，网络严重堵塞，HTTP和FTP服务完全停止，由此可见威力非同小可</span>

<span style="color: #0000ff;">。
4、-f 在数据包中发送“不要分段”标志。
在一般你所发送的数据包都会通过路由分段再发送给对方，加上此参数以后路由就不会</span>

<span style="color: #0000ff;">再分段处理。
5、-i TTL 指定TTL值在对方的系统里停留的时间。
此参数同样是帮助你检查网络运转情况的。
6、-v TOS 将“服务类型”字段设置为 tos 指定的值。
7、-r count 在“记录路由”字段中记录传出和返回数据包的路由。
在一般情况下你发送的数据包是通过一个个路由才到达对方的，但到底是经过了哪些路</span>

<span style="color: #0000ff;">由呢？通过此参数就可以设定你想探测经过的路由的个数，不过限制在了9个，也就是说你</span>

<span style="color: #0000ff;">只能跟踪到9个路由，如果想探测更多，可以通过其他命令实现。
C:＼&gt;ping -n 1 -r 9 202.96.105.101 （发送一个数据包，最多记录9个路由）
Pinging 202.96.105.101 with 32 bytes of data:
Reply from 202.96.105.101: bytes=32 time=10ms TTL=249
Route: 202.107.208.187 -&gt;
202.107.210.214 -&gt;
61.153.112.70 -&gt;
61.153.112.89 -&gt;
202.96.105.149 -&gt;
202.96.105.97 -&gt;
202.96.105.101 -&gt;
202.96.105.150 -&gt;
61.153.112.90
Ping statistics for 202.96.105.101:
Packets: Sent = 1, Received = 1, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 10ms, Maximum = 10ms, Average = 10ms
从上面我就可以知道从我的计算机到202.96.105.101一共通过了202.107.208.187 ，</span>

<span style="color: #0000ff;">202.107.210.214 , 61.153.112.70 , 61.153.112.89 , 202.96.105.149 , 202.96.105.97</span>

<span style="color: #0000ff;">这几个路由。
8、-s count 指定 count 指定的跃点数的时间戳。
此参数和-r差不多，只是这个参数不记录数据包返回所经过的路由，最多也只记录4个</span>

<span style="color: #0000ff;">。
9、-j host-list 利用 computer-list 指定的计算机列表路由数据包。连续计算机可</span>

<span style="color: #0000ff;">以被中间网关分隔（路由稀疏源）IP 允许的最大数量为 9。
10、-k host-list 利用 computer-list 指定的计算机列表路由数据包。连续计算机不</span>

<span style="color: #0000ff;">能被中间网关分隔（路由严格源）IP 允许的最大数量为 9。
11、-w timeout 指定超时间隔，单位为毫秒。
12、-t--连续对IP地址执行Ping命令，直到被用户以Ctrl+C中断。

[编辑本段]ping命令的其他技巧
在一般情况下还可以通过ping对方让对方返回给你的TTL值大小，粗略的判断目标主机</span>

<span style="color: #0000ff;">的系统类型是Windows系列还是UNIX/Linux系列，一般情况下Windows系列的系统返回的TTL</span>

<span style="color: #0000ff;">值在100-130之间，而UNIX/Linux系列的系统返回的TTL值在240-255之间，当然TTL的值在对</span>

<span style="color: #0000ff;">方的主机里是可以修改的，Windows系列的系统可以通过修改注册表以下键值实现：
[HKEY_LOCAL_MACHINE＼SYSTEM＼CurrentControlSet＼Services＼Tcpip＼Parameters]
"DefaultTTL"=dword:000000ff
255---FF
128---80
64----40
32----20
在网络没有问题，却无法PING通时可能有以下一些情况。
1.太心急。即网线刚插到交换机上就想Ping通网关，忽略了生成树的收敛时 间。当然</span>

<span style="color: #0000ff;">，较新的交换机都支持快速生成树，或者有的管理员干脆把用户端口（access port）的生</span>

<span style="color: #0000ff;">成树协议关掉，问题就解决了。
2.访问控制。不管中间跨越了多少跳，只要有节点（包括端节点）对ICMP进行了过滤，</span>

<span style="color: #0000ff;">Ping不通是正常的。最常见的就是防火墙的行为。
3.某些路由器端口是不允许用户Ping的。
还遇到过这样的情形，更为隐蔽。
1.网络因设备间的时延太大，造成ICMP echo报文无法在缺省时 间（2秒）内收到。时</span>

<span style="color: #0000ff;">延的原因有若干，比如线路（卫星网时延上下星为540毫秒），路由器处理时延，或路由设</span>

<span style="color: #0000ff;">计不合理造成迂回路径。使用扩展Ping，增加timed out时 间，可Ping通的话就属路由时延</span>

<span style="color: #0000ff;">太大问题。
2.引入NAT的场合会造成单向Ping通。NAT可以起到隐蔽内部地址的作用，当由内Ping外</span>

<span style="color: #0000ff;">时，可以Ping通是因为NAT表的映射关系存在，当由外发起Ping内网主机时，就无从查找边</span>

<span style="color: #0000ff;">界路由器的NAT表项了。
3.多路由负载均衡场合。比如Ping远端目的主机，成功的reply和timed out交错出现，</span>

<span style="color: #0000ff;">结果发现在网关路由器上存在两条到目的网段的路由，两条路由权重相等，但经查一条路由</span>

<span style="color: #0000ff;">存在问题。
4.IP地址分配不连续。地址规划出现问题象是在网络中埋了地雷，地址重叠或掩码划分</span>

<span style="color: #0000ff;">不连续都可能在Ping时出现问题。比如一个极端情况，A、B两台主机，经过多跳相连，A能</span>

<span style="color: #0000ff;">Ping通B的网关，而且B的网关设置正确，但A、B就是Ping不通。经查，在B的网卡上还设有</span>

<span style="color: #0000ff;">第二个地址，并且这个地址与A所在的网段重叠。
5.指定源地址的扩展Ping。登陆到路由器上，Ping远程主机，当ICMP echo request从</span>

<span style="color: #0000ff;">串行广域网接口发出去的时候，路由器会指定某个IP地址作为源IP，这个IP地址可能不是此</span>

<span style="color: #0000ff;">接口的IP或这个接口根本没有IP地址。而某个下游路由器可能并没有到这个IP网段的路由，</span>

<span style="color: #0000ff;">导致不能Ping通。可以采用扩展Ping，指定好源IP地址。
当主机网关和中间路由的配置认为正确时，出现Ping问题也是很普遍的现象。此时应该</span>

<span style="color: #0000ff;">忘掉"不可能"几个字，把Ping的扩展参数和反馈信息、traceroute、路由器debug、以及端</span>

<span style="color: #0000ff;">口镜像和Sniffer等工具结合起来进行分析。
比如，当A、B两台主机经过多跳路由器相连时，二者网关设置正确，在A上可以Ping通B</span>

<span style="color: #0000ff;">，但在B上不能Ping通A。可以通过在交换机做镜像，并用Sniffer抓包，来找出ICMP 报文终</span>

<span style="color: #0000ff;">止于何处，报文内容是什么，就可以发现ICMP报文中的源IP地址并非预期的那样，此时很容</span>

<span style="color: #0000ff;">易想象出可能是路由器的NAT功能使然，这样就能够逐步地发现一些被忽视的问题。而Ping</span>

<span style="color: #0000ff;">不通时的反馈信息是"destination_net_unreachable"还是"timed out"也是有区别的
[编辑本段]使用PING命令的各类反馈信息
Request timed out
[2]a.对方已关机，或者网络上根本没有这个地址：比如在上图中主机A中PING </span>

<span style="color: #0000ff;">192.168.0.7 ,或者主机B关机了，在主机A中PING 192.168.0.5 都会得到超时的信息。
b.对方与自己不在同一网段内，通过路由也无法找到对方，但有时对方确实是存在的，</span>

<span style="color: #0000ff;">当然不存在也是返回超时的信息。
c.对方确实存在，但设置了ICMP数据包过滤（比如防火墙设置）
怎样知道对方是存在，还是不存在呢，可以用带参数 -a 的Ping命令探测对方，如果能</span>

<span style="color: #0000ff;">得到对方的NETBIOS名称，则说明对方是存在的，是有防火墙设置，如果得不到，多半是对</span>

<span style="color: #0000ff;">方不存在或关机，或不在同一网段内。
d.错误设置IP地址
正常情况下，一台主机应该有一个网卡，一个IP地址，或多个网卡，多个IP地址（这些</span>

<span style="color: #0000ff;">地址一定要处于不同的IP子网）。但如果一台电脑的“拨号网络适配器”（相当于一块软网</span>

<span style="color: #0000ff;">卡）的TCP/IP设置中，设置了一个与网卡IP地址处于同一子网的IP地址，这样，在IP层协议</span>

<span style="color: #0000ff;">看来，这台主机就有两个不同的接口处于同一网段内。当从这台主机Ping其他的机器时，会</span>

<span style="color: #0000ff;">存在这样的问题：
A.主机不知道将数据包发到哪个网络接口，因为有两个网络接口都连接在同一网段。
B.主机不知道用哪个地址作为数据包的源地址。因此，从这台主机去Ping其他机器，IP</span>

<span style="color: #0000ff;">层协议会无法处理，超时后，Ping 就会给出一个“超时无应答”的错误信息提示。但从其</span>

<span style="color: #0000ff;">他主机Ping这台主机时，请求包从特定的网卡来，ICMP只须简单地将目的、源地址互换，并</span>

<span style="color: #0000ff;">更改一些标志即可，ICMP应答包能顺利发出，其他主机也就能成功Ping通这台机器了。
Destination host Unreachable
对方与自己不在同一网段内，而自己又未设置默认的路由，比如上例中A机中不设定默</span>

<span style="color: #0000ff;">认的路由，运行Ping 192.168.0.1.4就会出现“Destination host Unreachable”。
网线出了故障
这里要说明一下“destination host unreachable”和 “time out”的区别，如果所</span>

<span style="color: #0000ff;">经过的路由器的路由表中具有到达目标的路由，而目标因为其他原因不可到达，这时候会出</span>

<span style="color: #0000ff;">现“time out”，如果路由表中连到达目标的路由都没有，那就会出现“destination host </span>

<span style="color: #0000ff;">unreachable”。
Bad IP address
这个信息表示您可能没有连接到DNS服务器，所以无法解析这个IP地址，也可能是IP地</span>

<span style="color: #0000ff;">址不存在。
Source quench received
这个信息比较特殊，它出现的机率很少。它表示对方或中途的服务器繁忙无法回应。
Unknown host——不知名主机
这种出错信息的意思是，该远程主机的名字不能被域名服务器（DNS）转换成IP地址。</span>

<span style="color: #0000ff;">故障原因可能是域名服务器有故障，或者其名字不正确，或者网络管理员的系统与远程主机</span>

<span style="color: #0000ff;">之间的通信线路有故障。
No answer——无响应
这种故障说明本地系统有一条通向中心主机的路由，但却接收不到它发给该中心主机的</span>

<span style="color: #0000ff;">任何信息。故障原因可能是下列之一：中心主机没有工作；本地或中心主机网络配置不正确</span>

<span style="color: #0000ff;">；本地或中心的路由器没有工作；通信线路有故障；中心主机存在路由选择问题。
Ping 127.0.0.1：127.0.0.1是本地循环地址
如果本地址无法Ping通，则表明本地机TCP/IP协议不能正常工作。
no rout to host：网卡工作不正常。
transmit failed,error code：10043网卡驱动不正常。
unknown host name：DNS配置不正确。

利用PING来检查网络状态的方法：</span>

<span style="color: #0000ff;">
1.Ping本机IP
例如本机IP地址为：172.168.200.2。则执行命令Ping 172.168.200.2。如果网卡安装</span>

<span style="color: #0000ff;">配置没有问题，则应有类似下列显示：
Replay from 172.168.200.2 bytes=32 time&lt;10ms
Ping statistics for 172.168.200.2
Packets Sent=4 Received=4 Lost=0 0% loss
Approximate round trip times in milli-seconds
Minimum=0ms Maxiumu=1ms Average=0ms
如果在MS-DOS方式下执行此命令显示内容为：Request timed out，则表明网卡安装或</span>

<span style="color: #0000ff;">配置有问题。将网线断开再次执行此命令，如果显示正常，则说明本机使用的IP地址可能与</span>

<span style="color: #0000ff;">另一台正在使用的机器IP地址重复了。如果仍然不正常，则表明本机网卡安装或配置有问题</span>

<span style="color: #0000ff;">，需继续检查相关网络配置。
2.Ping网关IP
假定网关IP为：172.168.6.1，则执行命令Ping 172.168.6.1。在MS-DOS方式下执行此</span>

<span style="color: #0000ff;">命令，如果显示类似以下信息：
Reply from 172.168.6.1 bytes=32 time=9ms TTL=255
Ping statistics for 172.168.6.1
Packets Sent=4 Received=4 Lost=0
Approximate round trip times in milli-seconds
Minimum=1ms Maximum=9ms Average=5ms
则表明局域网中的网关路由器正在正常运行。反之，则说明网关有问题。
3.Ping远程IP
这一命令可以检测本机能否正常访问Internet。比如本地电信运营商的IP地址为：</span>

<span style="color: #0000ff;">202.102.48.141。在MS-DOS方式下执行命令：Ping 202.102.48.141，如果屏幕显示：
Reply from 202.102.48.141 bytes=32 time=33ms TTL=252
Reply from 202.102.48.141 bytes=32 time=21ms TTL=252
Reply from 202.102.48.141 bytes=32 time=5ms TTL=252
Reply from 202.102.48.141 bytes=32 time=6ms TTL=252
Ping statistics for 202.102.48.141
Packets Sent=4 Received=4 Lost=0 0% loss
Approximate round trip times in milli-seconds
Minimum=5ms Maximum=33ms Average=16ms
则表明运行正常，能够正常接入互联网。反之，则表明主机文件（windows/host）存在</span>

<span style="color: #0000ff;">问题。
ping命令的用法
验证与远程计算机的连接。该命令只有在安装了 TCP/IP 协议后才可以使用。
ping [-t] [-a] [-n count] [-l length] [-f] [-i ttl] [-v tos] [-r count] [-s </span>

<span style="color: #0000ff;">count] [[-j computer-list] | [-k computer-list]] [-w timeout] destination-list
参数
-t
Ping 指定的计算机直到中断。
-a
将地址解析为计算机名。
-n count
发送 count 指定的 ECHO 数据包数。默认值为 4。
-l length
发送包含由 length 指定的数据量的 ECHO 数据包。默认为 32 字节；最大值是 </span>

<span style="color: #0000ff;">65,527。
-f
在数据包中发送“不要分段”标志。数据包就不会被路由上的网关分段。
-i ttl
将“生存时间”字段设置为 ttl 指定的值。
-v tos
将“服务类型”字段设置为 tos 指定的值。
-r count
在“记录路由”字段中记录传出和返回数据包的路由。count 可以指定最少 1 台，最</span>

<span style="color: #0000ff;">多 9 台计算机。
-s count
指定 count 指定的跃点数的时间戳。
-j computer-list
利用 computer-list 指定的计算机列表路由数据包。连续计算机可以被中间网关分隔</span>

<span style="color: #0000ff;">（路由稀疏源）IP 允许的最大数量为 9。
-k computer-list
利用 computer-list 指定的计算机列表路由数据包。连续计算机不能被中间网关分隔</span>

<span style="color: #0000ff;">（路由严格源）IP 允许的最大数量为 9。
-w timeout
指定超时间隔，单位为毫秒。
destination-list
指定要 ping 的远程计算机

Ping相关环境讯息</span>

<span style="color: #0000ff;">
适用环境：WIN95/98/2000/NT
使用格式：ping [-t] [-a] [-n count] [-l size]
参数介绍：
-t 让用户所在的主机不断向目标主机发送数据
-a 以IP地址格式来显示目标主机的网络地址
-n count 指定要ping多少次，具体次数由后面的count来指定
-l size 指定发送到目标主机的数据包的大小
主要功能：用来测试一帧数据从一台主机传输到另一台主机所需的时间，从而判断主响</span>

<span style="color: #0000ff;">应时间。
详细介绍：
该命令主要是用来检查路由是否能够到达，由于该命令的包长非常小，所以在网上传递</span>

<span style="color: #0000ff;">的速度非常快，可以快速地检测你要去的站点是否可达，一般你在去某一站点时可以先运行</span>

<span style="color: #0000ff;">一下该命令看看该站点是否可达。如果执行Ping不成功，则可以预测故障出现在以下几个方</span>

<span style="color: #0000ff;">面：网线是否连通，网络适配器配置是否正确，IP地址是否可用等；如果执行Ping成功而网</span>

<span style="color: #0000ff;">络仍无法使用，那么问题很可能出在网络系统的软件配置方面，Ping成功只能保证当前主机</span>

<span style="color: #0000ff;">与目的主机间存在一条连通的物理路径。它的使用格式是在命令提示符下键入：Ping　IP地</span>

<span style="color: #0000ff;">址或主机名，执行结果显示响应时间，重复执行这个命令，你可以发现Ping报告的响应时间</span>

<span style="color: #0000ff;">是不同的。具体的ping命令后还可跟好多参数，你可以键入ping后回车其中会有很详细的说</span>

<span style="color: #0000ff;">明。

防范被Ping与封闭端口</span>

<span style="color: #0000ff;">
随着学校校园网越来越多人使用，用户对网络知识认知的提高，很多人在网上下载一些</span>

<span style="color: #0000ff;">黑客工具或者用Ping命令，进行扫描端口、IP寻找肉机，带来很坏的影响。
Ping命令它可以向你提供的地址发送一个小的数据包，然后侦听这台机器是否有“回答</span>

<span style="color: #0000ff;">”。查找现在哪些机器在网络上活动。使用Ping入侵即是ICMP 入侵，原理是通过Ping在一</span>

<span style="color: #0000ff;">个时段内连续向计算机发出大量请求使得计算机的CPU占用率居高不下达到100%而系统死机</span>

<span style="color: #0000ff;">甚至崩溃。基于此，写这篇IP安全策略防Ping文章以保障自己的系统安全。
其实防Ping安装和设置防火墙也可以解决，但防火墙并不是每一台电脑都会去装，要考</span>

<span style="color: #0000ff;">虑资源占用还有设置技巧。如果你安装了防火墙但没有去修改、添加IP规则那一样没用。有</span>

<span style="color: #0000ff;">些配置不是很高为免再给防火墙占用资源用手工在自己系统中设置安全略是一个上上的办法</span>

<span style="color: #0000ff;">。
下面就写下具体创建过程：
（一）创建IP安全策略
1、依次单击“开始→控制面板→管理工具→本地安全策略”，打开“本地安全设置”</span>

<span style="color: #0000ff;">，右击该对话框左侧的“IP安全策略，在本地计算机”选项，执行“创建IP安全策略”命令</span>

<span style="color: #0000ff;">。（之间有些简单的点击下一步之类的过程省略不写）
2、在出现的“默认响应规则身份验证方法”对话框中我们选中“此字符串用来保护密</span>

<span style="color: #0000ff;">钥交换（预共享密钥）”选项，然后在下面的文字框中任意键入一段字符串。（如“禁止 </span>

<span style="color: #0000ff;">Ping”）
3、完成了IP安全策略的创建工作后在“IP筛选器列表”窗口中单击“添加”按钮，此</span>

<span style="color: #0000ff;">时将会弹出“IP筛选器向导”窗口，我们单击“下一步”，此时将会弹出“IP通信源”页面</span>

<span style="color: #0000ff;">，在该页面中设置“源地址”为“我的IP地址”：“目标地址”为“任何IP地址”，任何IP</span>

<span style="color: #0000ff;">地址的计算机都不能Ping你的机器。
在“筛选器属性”中可封闭端口。比如封闭TCP协议的135端口：在“选择协议类型”的</span>

<span style="color: #0000ff;">下拉列表中选择“TCP”，然后在“到此端口”下的文本框中输入“135”，点击“确定”按</span>

<span style="color: #0000ff;">钮，这样就添加了一个屏蔽 TCP 135（RPC）端口的筛选器，它可以防止外界通过135端口连</span>

<span style="color: #0000ff;">上你的电脑。重复可封闭TCP UDP等自己认为需要封闭的端口。这里不一一写出。
4、依次单击“下一步”→“完成”，此时，你将会在“IP筛选器列表”看到刚刚创建</span>

<span style="color: #0000ff;">的筛选器，将其选中后单击“下一步”，我们在出现的“筛选器操作”页面中设置筛选器操</span>

<span style="color: #0000ff;">作为“需要安全”选项。
（二）指派IP安全策略
安全策略创建完毕后并不能马上生效，我们还需通过“指派”功能令其发挥作用。方法</span>

<span style="color: #0000ff;">是：在“控制台根节点”中右击“新的IP安全策略”项，然后在弹出的右键菜单中执行“指</span>

<span style="color: #0000ff;">派”命令，即可启用该策略。
至此，这台主机已经具备了拒绝其他任何机器Ping自己IP地址的功能，不过在本地仍然</span>

<span style="color: #0000ff;">能够Ping通自己。经过这样的设置之后，所有用户（包括管理员）都不能在其他机器上对此</span>

<span style="color: #0000ff;">服务器进行Ping操作。从此你再也不用担心被Ping威胁。如果再把一些黑客工具、木马常探</span>

<span style="color: #0000ff;">寻的端口封闭那你的系统就更加固若金汤了。
Ping的工作过程及单向Ping通的原因
当网络出现问题时，我们最常用的测试工具就是“Ping”命令了。但有时候我们会碰到</span>

<span style="color: #0000ff;">单方向Ping通的现象，例如通过HUB或一根交叉线连接的在同一个局域网内的电脑A、 B，在</span>

<span style="color: #0000ff;">检查它们之间的网络连通性时，发现从主机A Ping 主机B正常而从主机B Ping 主机A时，出</span>

<span style="color: #0000ff;">现“超时无应答”错误。为什么呢?
要知道这其中的奥秘，我们有必要来看看Ping命令的工作过程到底是怎么样的。
假定主机A的IP地址是192.168.1.1，主机B的IP地址是192.168.1.2，都在同一子网内，</span>

<span style="color: #0000ff;">则当你在主机A上运行“Ping 192.168.1.2”后，都发生了些什么呢?
首先，Ping命令会构建一个固定格式的ICMP请求数据包，然后由ICMP协议将这个数据包</span>

<span style="color: #0000ff;">连同地址“192.168.1.2”一起交给IP层协议(和ICMP一样，实际上是一组后台运行的进程)</span>

<span style="color: #0000ff;">，IP层协议将以地址“192.168.1.2”作为目的地址，本机IP地址作为源地址，加上一些其</span>

<span style="color: #0000ff;">他的控制信息，构建一个IP数据包，并在一个映射表中查找出IP地址192.168.1.2所对应的</span>

<span style="color: #0000ff;">物理地址(也叫MAC地址，熟悉网卡配置的朋友不会陌生，这是数据链路层协议构建数据链路</span>

<span style="color: #0000ff;">层的传输单元——帧所必需的)，一并交给数据链路层。后者构建一个数据帧，目的地址是</span>

<span style="color: #0000ff;">IP层传过来的物理地址，源地址则是本机的物理地址，还要附加上一些控制信息，依据以太</span>

<span style="color: #0000ff;">网的介质访问规则，将它们传送出去。
主机B收到这个数据帧后，先检查它的目的地址，并和本机的物理地址对比，如符合，</span>

<span style="color: #0000ff;">则接收;否则丢弃。接收后检查该数据帧，将IP数据包从帧中提取出来，交给本机的IP层协</span>

<span style="color: #0000ff;">议。同样，IP层检查后，将有用的信息提取后交给ICMP协议，后者处理后，马上构建一个</span>

<span style="color: #0000ff;">ICMP应答包，发送给主机A，其过程和主机A发送ICMP请求包到主机B一模一样。
从Ping的工作过程，我们可以知道，主机A收到了主机B的一个应答包，说明两台主机之</span>

<span style="color: #0000ff;">间的去、回通路均正常。也就是说，无论从主机A到主机B，还是从主机B到主机A，都是正常</span>

<span style="color: #0000ff;">的。那么，是什么原因引起只能单方向Ping通的呢?
一、安装了个人防火墙
在共享上网的机器中，出于安全考虑，大部分作为服务器的主机都安装了个人防火墙软</span>

<span style="color: #0000ff;">件，而其他作为客户机的机器则一般不安装。几乎所有的个人防火墙软件，默认情况下是不</span>

<span style="color: #0000ff;">允许其他机器Ping本机的。一般的做法是将来自外部的ICMP请求报文滤掉，但它却对本机出</span>

<span style="color: #0000ff;">去的ICMP请求报文，以及来自外部的ICMP应答报文不加任何限制。这样，从本机Ping其他机</span>

<span style="color: #0000ff;">器时，如果网络正常，就没有问题。但如果从其他机器Ping这台机器，即使网络一切正常，</span>

<span style="color: #0000ff;">也会出现“超时无应答”的错误。
大部分的单方向Ping通现象源于此。解决的办法也很简单，根据你自己所用的不同类型</span>

<span style="color: #0000ff;">的防火墙，调整相应的设置即可。
二、错误设置IP地址
正常情况下，一台主机应该有一个网卡，一个IP地址，或多个网卡，多个IP地址(这些</span>

<span style="color: #0000ff;">地址一定要处于不同的IP子网)。但对于在公共场所使用的电脑，特别是网吧，人多手杂，</span>

<span style="color: #0000ff;">其中不泛有“探索者”。曾有一次两台电脑也出现了这种单方向Ping通的情况，经过仔细检</span>

<span style="color: #0000ff;">查，发现其中一台电脑的“拨号网络适配器”(相当于一块软网卡)的TCP/IP设置中，设置了</span>

<span style="color: #0000ff;">一个与网卡IP地址处于同一子网的IP地址，这样，在IP层协议看来，这台主机就有两个不同</span>

<span style="color: #0000ff;">的接口处于同一网段内。当从这台主机Ping其他的机器时，会存在这样的问题:
(1)主机不知道将数据包发到哪个网络接口，因为有两个网络接口都连接在同一网段;
(2)主机不知道用哪个地址作为数据包的源地址。因此，从这台主机去Ping其他机器，</span>

<span style="color: #0000ff;">IP层协议会无法处理，超时后，Ping 就会给出一个“超时无应答”的错误信息提示。但从</span>

<span style="color: #0000ff;">其他主机Ping这台主机时，请求包从特定的网卡来，ICMP只须简单地将目的、源地址互换，</span>

<span style="color: #0000ff;">并更改一些标志即可，ICMP应答包能顺利发出，其他主机也就能成功Ping通这台机器了
尊重词条编辑者 不要为了积分去掉原本编辑者的连接 !</span>

</div>