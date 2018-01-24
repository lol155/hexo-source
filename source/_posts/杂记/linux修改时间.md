---
title: linux修改时间 
categories: 学习
tags:
  - linux
toc: true
date: 2018-01-24 15:03:02
scaffolds:
---
linux修改时间
- 修改时区
> cp /usr/share/zoneinfo/UTC /etc/localtime 
```
[root@localhost ~]# cp /usr/share/zoneinfo/UTC /etc/localtime 
cp：是否覆盖“/etc/localtime”? y 
[root@localhost ~]# date 
2012年 11月 02日 星期五 00:07:30 UTC
修改为中国的东八区
```

- 配置新的时间
```
# vi /etc/sysconfig/clock
ZONE="Asia/Shanghai"
UTC=false
ARC=false
```
* 安装ntp
```
yum install -y ntp
```
* 设置时间同步
```
ntpdate 210.72.145.44
```



参考链接 [Linux服务器同步网络时间](http://www.linuxidc.com/Linux/2017-03/141745.htm)