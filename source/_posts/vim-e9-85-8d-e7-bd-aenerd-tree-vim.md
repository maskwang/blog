---
title: vim配置NERD_tree.vim
id: 335
categories:
  - Linux
  - 技术专栏
date: 2011-10-12 23:48:20
tags:
---

安装[NERD_tree.vim](http://www.vim.org/scripts/script.php?script_id=1658)。 找到NERD_tree.zip，下载。把解压缩后的NERD_tree.vim复制到＄.vim/plugin目录下，把NERD_tree.txt复 制到＄.vim/doc目录下。然后打开gvim，在命令窗口中键入“gvim“，在gvim窗口中，按ESC键转到命令行模式，：NERDTree回 车，在gvim窗口的左侧就会出现树形的窗口。安装成功。

你可以双击文件在当前的窗口打开，也可以中键点击文件，在一个新的分割窗口内打开，也可以用 t 键，在一个新的标签页打开文件，C

键可以把当前的目录作为顶极目录，? 就可以得到一个常用命令手册，更详细的命令和功能可以查看 NERD tree 的帮助： :help

NERD_tree.txt 。

o 打开关闭文件或者目录 t 在标签页中打开 T 在后台标签页中打开 ! 执行此文件 p 到上层目录 P 到根目录 K 到第一个节点 J 到最后一个节点 u 打开上层目录 m 显示文件系统菜单（添加、删除、移动操作） ? 帮助 q 关闭Shift+R刷新目录树