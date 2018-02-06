---
title: 大数据02-02 linux命令继续学习 yum
categories: 大数据学习笔记
tags:
  - 大数据
  - linux
  - centos7
  - linux命令
toc: true
date: 2017-11-01 23:50:39
scaffolds:
---
# 1. YUM的常用命令

安装httpd并确认安装
```
yum instll -y httpd
```
列出所有可用的package和package组
```
yum list
```
<!--more-->
清除所有缓冲数据
```
yum clean all
```
列出一个包所有依赖的包
```
yum deplist httpd
```
删除httpd
```
yum remove httpd
```
# 2. 配置本地yum源
cd /etc/yum.repos.d 

# 3. 安装jdk
解压安装包
> tar -zxvf jdk-7u45-linux-x64.tar.gz -C apps/

然后修改环境变量
> vi /etc/profile

在文件最后添加
```
export JAVA_HOME=/root/apps/jdk1.7.0_45
export PATH=$PATH:$JAVA_HOME/bin
```

保存退出

然后重新加载环境变量
> source /etc/profile

# 4. 装mysql
# 5. 安装tomcat
1. 上传tomcat包
2. 解压
3. 启动
4. 测试访问
