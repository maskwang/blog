---
title: 'SVN合并过程详解 '
id: 292
categories:
  - Svn
  - 版本控制
date: 2011-08-26 06:02:24
tags:
---

SVN对于项目管理的神秘面纱正在被我一步步揭开，今天讲解SVN进行项目合并的流程，很细致哟...

第一步：将要合并的branches导入到本地（在eclipse内对Import），然后合并。合并时From为当前Branches来源caboose，To为trunk,确定即可。
注：1.合并的结果将caboose到trunk的变化合并的到本地，这样本地为最新代码。
第二步：提交第一步的branches，注释可写为：rebase from caboose-2010_35@HEAD to trunk@58570
第三步：将trunk导入到本地，然后合并。合并是From为当前trunk，To为前两步中提交的branches，确定即可。
第四步：更新。第三步操作完成后，更新trunk，这样SVN服务器上的trunk就与本地一致，都为最新的。注释可写为