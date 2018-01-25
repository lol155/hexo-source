---
title: linux 程序down 问题排查
categories: 学习
tags:
  - linux
toc: true
date: 2018-01-24 15:03:02
scaffolds:
---
# linux系统有自我保护机制
如果分配的内存用完了,会杀掉进程.  
可以查看日志  
```
/var/log/messages
```
然后通过进程ID 进行查询  ,或者 `oom-killer` 来查询
![linux 程序down 问题排查-20171226102834](http://ovasdkxqr.bkt.clouddn.com/image/work/linux%20程序down%20问题排查-20171226102834.png)

## 内存不够
可以配置虚拟内存,解决内存高峰时内存不够的问题.

# 线程挂掉用jstack分析线程栈
可以使用 jstack 查看进程信息

# 其他命令
## lsof -n (学习)
