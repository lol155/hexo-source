---
title: linux笔记-修改语言
categories: 学习
tags:
  - linux
  - centos7
toc: true
date: 2017-11-05 23:39:47
scaffolds:
---
# 版本
centos7
# 查看系统是否有安装中文语言包 （列出所有可用的公共语言环境的名称，包含有zh_CN）
```
[vagrant@mysql1 ~]$ locale -a |grep "zh_CN"
zh_CN
zh_CN.gb18030
zh_CN.gb2312
zh_CN.gbk
zh_CN.utf8
```
若发现以上几项，说明系统已安装中文语言包，无需再安装
<!-- more -->
# 安装中文包
```
root@iZj6cbstl2n6r280a27eppZ tmp]# yum groupinstall "fonts"
```
# 修改i18n国际化和locale.conf本土化配置文件
## 先查看系统语言环境
```
[vagrant@mysql1 ~]$ locale
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
```
## 修改配置文件
```
 vi /etc/locale.conf 
 或
 vi /etc/sysconfig/i18n (有些帖子上说修改这个,但是我的linux没有这个文件)
```

虽然安装了中文语言包但本机的语言环境并不是中文，先修改i18n配置文件

```
[root@iZj6cbstl2n6r280a27eppZ sysconfig]# vim /etc/sysconfig/i18n

LANG="zh_CN.UTF-8"
LC_ALL="zh_CN.UTF-8"

[root@iZj6cbstl2n6r280a27eppZ sysconfig]# source /etc/sysconfig/i18n

[root@iZj6cbstl2n6r280a27eppZ sysconfig]# vim /etc/locale.conf

LANG="zh_CN.UTF-8"

 [root@iZj6cbstl2n6r280a27eppZ sysconfig]# source   /etc/locale.conf
```