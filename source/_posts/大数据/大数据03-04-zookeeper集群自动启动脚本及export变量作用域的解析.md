---
title: 大数据03-04-zookeeper集群自动启动脚本及export变量作用域的解析
categories: 学习
tags:
  - 大数据
  - zookeeper
toc: true
date: 2017-11-21 23:22:14
scaffolds:
---
1. export A=1 定义的变量，会对自己所在的shell进程及其子进程生效
2. B=1 定义的变量，只对自己所在的shell进程生效
3. 在script.sh中定义的变量，在当前登录的shell进程中 source script.sh 时，脚本中定义的变量也会进入当前登录的进程
<!-- more -->

```bash
#!/bin/sh
echo "start zkServer..."
for i in 1 2 3
do 
ssh zk$i "source /etc/profile;/home/vagrant/apps/zookeeper/bin/zkServer.sh start"
done 
```
1. 提示输入密码，配置免密登录
```
ssh-keygen 
ssh-copy-id zk1
```
2. 可以放到/root/bin 目录下，该目录直接就在环境变量中，不需要配置，可以在其他位置执行了
