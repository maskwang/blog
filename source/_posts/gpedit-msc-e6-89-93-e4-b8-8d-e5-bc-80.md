---
title: gpedit.msc打不开
id: 158
categories:
  - 其它
  - 技术专栏
date: 2011-08-22 23:06:22
tags:
---

<div id="blog_text">

问：

gpedit.msc打不开
<div></div>
<div>
<pre>按网上方法
在运行里输入mmc，然后进文件的添加删除单元组策略
但为什么我找不到啊</pre>
<pre></pre>
<pre>答：</pre>
<pre></pre>
<pre>看来我今天还得再说一次这个问题
我估计你的windows是家庭版的   你点开始试下  左面是不是有竖着的蓝底白字，写的Windows XP home Ediation??
  有的话证明你的windows是家庭版的  是没有组策略的，专业版的才有，解决这个问题的方法就是把专业版XP的组策略文件复制过来
我在其他提问里回答过这个问题了  今天就再说一次~

1、将XP专业版的“C:\WINDOWS\system32”文件夹中的gpedit.msc、fde.dll、gpedit.dll、gptext.dll、wsecedit.dll文件复制到HOME版的“C:\WINDOWS\system32”文件夹中。 
2、在“开始--运行”中依次运行以下命令：“regsvr32 fde.dll”、“regsvr32 gpedit.dll”、“regsvr32 gptext.dll”、“regsvr32 wsecedit.dll”分别注册这4个动态数据库。 
3、将XP专业版的“C:\WINDOWS\INF”文件夹中的所有*.adm文件复制替换到HOME版的“C:\WINDOWS\INF”文件夹中。 
4、最后单击“开始--运行”，输入“gpedit.msc”便可以启动组策略了，启动的组策略就是正常的、什么都有的组策略~~~

OK成功</pre>
<pre></pre>
<pre></pre>
<pre>第二种方法：</pre>
<pre></pre>
<pre>在注册表中打开HKEY-LOCAL-MACHINE＼SYSTEM＼ CurrentControlSet＼Services＼Cdrom子项，双击右边窗口中的“Autorun”，将其值设为0，即可关闭自动播放功能，如果要恢复自动播放功能，只要将其值改回1即可

如过觉得改注册表麻烦，还有一个办法，点击 开始 运行 （或者使用快捷键 win+r）在对话框中  输入 gpedit.msc 
然后打开 计算机配置——  管理模板 ——系统 
双击 关闭自动播放 设置中 选 已启动 确定即可。</pre>
</div>
</div>