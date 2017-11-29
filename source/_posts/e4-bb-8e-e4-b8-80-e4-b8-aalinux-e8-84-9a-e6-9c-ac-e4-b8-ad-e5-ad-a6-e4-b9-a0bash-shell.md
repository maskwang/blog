---
title: 从一个linux脚本中学习bash shell
id: 424
categories:
  - Linux
  - 技术专栏
date: 2011-10-27 15:00:34
tags:
---

<table>
<tbody>
<tr>
<td>
<div id="blog_text">

以下是一个resin启动脚本,其中可以学到很多bash shell的知识点

#!/bin/sh                   // bash shell脚本
#
# Linux startup script for Resin   //#注释
# chkconfig: 345 85 15
# description: Resin is a Java Web server.
# processname: wrapper.pl
#
# To install, configure this file as needed and copy init.resin
# to /etc/rc.d/init.d as resin. Then use "# /sbin/chkconfig resin reset"
#
JAVA_HOME=/usr/local/java/jdk    // =号赋值
RESIN_HOME=/usr/local/resin

export JAVA_HOME RESIN_HOME    // export 定义全局变量

JAVA=$JAVA_HOME/bin/java       // $ 代表变量
#
# If you want to start the entire Resin process as a different user,
# set this to the user name. If you need to bind to a protected port,
# e.g. port 80, you can't use USER, but will need to use bin/resin.
#
USER=
#
# Set to the server id to start
#
#SERVER="-server app-a"
#
ARGS="-resin-home $RESIN_HOME $SERVER"

if test -r /lib/lsb/init-functions; then   // 判断语句     test -r /lib/lsb/init-functions 条件文件存在并可读
. /lib/lsb/init-functions                  // . 代表源 同source /lib/lsb/init-functions
else                                       //如果不符合上面的条件

log_daemon_msg () {                     //函数
if [ -z "$1" ]; then                 // -z "$1" 变量$1长度为0   // $1 脚本执行时传过来的第一个参数
return 1                       //返回
fi

if [ -z "$2" ]; then
echo -n "$1:"              // -n 参数 输出不换行
return
fi

echo -n "$1: $2"
}

log_end_msg () {
[ -z "$1" ] &amp;&amp; return 1    // [ -z "$1" ]为真,执行 return 1

if [ $1 -eq 0 ]; then       //比较,-eq等于
echo " ."
else
echo " failed!"
fi

return $1
}

fi

case "$1" in                 //case 选择语句
start)                     //脚本第一个参数为start
log_daemon_msg "Starting resin"    //调用函数，传递参数
if test -n "$USER"; then           //条件：变量USER长度不为0
su $USER -c "$JAVA -jar $RESIN_HOME/lib/resin.jar $ARGS start" 1&gt;/dev/null 2&gt;/dev/null  //su 以其它用户执行 -c执行命令后恢复身份
else
$JAVA -jar $RESIN_HOME/lib/resin.jar $ARGS start 1&gt;/dev/null 2&gt;/dev/null     //1&gt;dev/null 执行正常信息输出到空文件 2&gt;错误信息
fi
log_end_msg $?      // $? 上一条命令执行的结果 0表示成功，更多请查看其它文章
;;
stop)
log_daemon_msg "Stopping resin"
if test -n "$USER"; then
su $USER -c "$JAVA -jar $RESIN_HOME/lib/resin.jar $ARGS stop" 1&gt;/dev/null 2&gt;/dev/null
else
$JAVA -jar $RESIN_HOME/lib/resin.jar $ARGS stop 1&gt;/dev/null 2&gt;/dev/null
fi
log_end_msg $?
;;
restart)
$0 stop       //$0 本脚本名
$0 start
;;
*)                  //除start stop restart外
echo "Usage: $0 {start|stop|restart}"
exit 1  //1表示有错误
esac

exit 0  //0表示没有错误

</div></td>
</tr>
</tbody>
</table>