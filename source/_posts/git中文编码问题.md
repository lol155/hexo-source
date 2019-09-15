---
title: git中文编码问题
categories: 学习
tags:
  - git
toc: true
date: 2019-09-15 20:24:10
scaffolds:
---

git中文编码问题解决
<!--more-->
这个问题是由于git默认会被utf-8文件名进行转码，需要设置

```bash
git config --global core.quotepath false
```