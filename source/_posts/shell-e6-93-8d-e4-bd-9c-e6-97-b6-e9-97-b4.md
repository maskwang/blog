---
title: shell操作时间
id: 409
categories:
  - Linux
  - 技术专栏
date: 2011-10-21 16:27:35
tags:
---

date +%Y%m%d -d "2 day ago"
date +%Y%m%d -d "2 week ago"
date +%Y%m%d -d "2 month ago"
date +%Y%m%d -d "2 year ago"
date -d "yesterday"

昨天的命令是：
yesterdayformat=`date --date='yesterday' "+%Y-%m-%d_%H:%M:%S"`
echo $yesterdayformat
输出格式是：
2006-03-30_08:39:54

明天的命令是：
tomorrowformat=`date --date='tomorrow' "+%Y-%m-%d_%H:%M:%S"`
echo $tomorrowformat
输出格式是：
2006-04-01_08:41:29

在Linux下，得到N天以前或以后的日期格式：
#date –I –d ‘-n day’   (可以得到N天前的日期，格式为YYYY-MM-DD)
#date –d ‘-n day’ “＋％Y%m%d”       (可以得到你天前的日期，格式为YYYYMMDD)
#date –I –d ‘+n day’   (可以得到N天后的日期，格式为YYYY-MM-DD)
#date –d ‘+n day’ “＋％Y%m%d”       (可以得到你天后的日期，格式为YYYYMMDD)

CURTIME=`date +"%Y-%m-%d %H:%M:%S"` #当前的系统时间 2007-10-04 14:34:00
LASTLINE=$(tail -1 success.moni) #获取文件的最后时间 2007-10-04 14:30:00
echo "lasttime "$LASTLINE
echo "Systime "$CURTIME
Sys_data=`date -d   "$CURTIME" +%s` #把当前时间转化为Linux时间
In_data=`date -d   "$LASTLINE" +%s`
interval=`expr $Sys_data - $In_data`   #计算2个时间的差
echo $In_data
echo $Sys_data
echo $interval
if [ $interval -gt 600 ] ; then
echo "need   restart"
exit 0
fi

echo "need't restart"