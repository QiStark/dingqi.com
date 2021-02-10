---
title: git 学习
author: dingqi
date: '2021-02-10'
slug: git-学习
categories:
  - git
tags: []
---
一. git 是什么？
----------------
Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

二.git 常见命令
----------------
1. git init:把目录变成Git可以管理的仓库
2. git add file:把文件添加到仓库
3. git commit -m "xxx" 把文件提交到仓库
4. git status:查看仓库状态
5. git diff file：查看修改的地方
6. git log:查看提交日志
7. 简化输出: 
``` git log --pretty=oneline ```
8. 还原文件到上一个版本，HEAD表示当前，上上版本为```HEAD^```: ```git reset --hard HEAD^```
9. ```git reset --hard xxx``` : 退回指定的版本,commit id用git log查看
10. git reflog:查看每一次命令,确定回到未来的版本


