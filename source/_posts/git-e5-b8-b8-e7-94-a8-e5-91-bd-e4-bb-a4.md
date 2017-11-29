---
title: git常用命令
tags:
  - git
  - 命令
id: 630
categories:
  - Git
  - 技术专栏
  - 版本控制
date: 2012-08-30 14:18:03
---

1.设置git的用户和邮箱
git config --global user.name "maskwang"
git config --global user.email "mask.wang.cn@gmail.com"<!--more-->

2.生成公共的key
ssh-keygen -t rsa

3.拉取仓库
git clone XXX(仓库地址)

4.提交代码
git add .
git push origin master

5.分支的创建和合并
# git branch local
# git checkout local 切换到local

# ... 开发local分支

与master分支合并
# git checkout master
# git merge local
# git branch -d local 合并完后删除local

6.从服务器更新
git pull 从服务器更新

7.提交代码
git commit -m "更新说明"

8.查看其他成员更新日志
git log 查看其他成员更新日志

9.更新服务器上的项目文件
$git remote add origin git@github.com:username/workName.git