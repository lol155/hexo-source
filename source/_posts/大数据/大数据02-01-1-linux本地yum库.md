---
title: 大数据02-01-1 linux本地yum库
categories: 大数据学习笔记
tags:
  - 大数据
  - linux
  - centos7
toc: true
date: 2017-11-14 21:58:01
scaffolds:
---
# 1. 本地yum仓库的安装配置
## 1.1. 两种方式：  
### 1.1.1. 每一台机器都配一个本地文件系统上的yum仓库 file:///packege/path/
### 1.1.2. 在局域网内部配置一台节点(server-base)的本地文件系统yum仓库，然后将其发布到web服务器中，其他节点就可以通过http://server-base/pagekege/path/
<!-- more -->

- 制作流程：  
- 先挑选一台机器mini4，挂载一个系统光盘到本地目录/mnt/cdrom，
- 然后启动一个httpd服务器，
- 将/mnt/cdrom 软连接到httpd服务器的/var/www/html目录中 (cd /var/www/html; ln -s /mnt/cdrom ./centos )
- 然后通过网页访问测试一下：  http://mini4/centos   会看到光盘的目录内容

至此：网络版yum私有仓库已经建立完毕  
剩下就是去各台yum的客户端配置这个http地址到repo配置文件中
			

			

			
# 2. 无论哪种配置，都需要先将光盘挂在到本地文件目录中
> mount -t iso9660 /dev/cdrom   /mnt/cdrom

为了避免每次重启后都要手动mount，可以在/etc/fstab中加入一行挂载配置，即可自动挂载
```
vi  /etc/fstab
/dev/cdrom              /mnt/cdrom              iso9660 defaults        0 0		
```	

# 3. minimal安装的系统出现的问题：缺各种命令，安装软件时缺各种依赖

scp命令都没有：yum install -y openssh-clients
每台机器上都要安装才行


			