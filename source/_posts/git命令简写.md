---
title: git命令简写
categories: 学习
tags:
  - git
toc: true
date: 2019-09-15 20:24:40
scaffolds:
---

简单记录 git 设置简写

<!-- more -->

## 1. 方法一:git文件修改设置简写命令
修改~/.gitconfig文件

添加
```
[alias]
c = checkout 
st = status
cm = commit -m
br = branch 
```

## 2. 方法二:修改windows文件设置简写
1. 新建 ~/.bashrc文件
2. 编辑文档
```
alias ga='git add'
alias gaa='git add --all'
alias gapa='git add --patch'
```
3. 保存并source ~/.bashrc即可
