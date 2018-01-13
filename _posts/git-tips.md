---
title: git_tips
date: 2018-01-10 13:23:02
tags:
---
命令行查看版本变更常用命令
git log  ./ 当前目录变更版本列表
git show  <commit-hashId> 便可以显示某次提交的修改内容

1. git log filename
可以看到fileName相关的commit记录
2. git log -p filename
可以显示每次提交的diff
3. 只看某次提交中的某个文件变化，可以直接加上fileName
git show commit-id filename
4.根据commit-id查看某个提交
git show -s --pretty=raw  id(59047cce6eeb2d8fd9fa361e01dbb88d9a37cf4e)
5.借助可视化工具 如 sourceTree 在最后一次修改的记录上 右键选中文件 查看历史修改
6.git log 的常用选项
注：filename （绝对路径） 或 （先进入此文件所在的目录下，当前文件名）

