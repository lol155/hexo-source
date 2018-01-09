---
title: 大数据02-01 linux命令继续学习.md
categories: 大数据学习笔记
tags: [大数据,linux,linux命令]
toc: true
date: 2017-10-29 22:30:38
scaffolds:
---
# vmare克隆后修改地址问题
解决克隆后eth0不见的问题

直接修改  /etc/sysconfig/network-script/ifcfg-eth0 
删掉UUID  HWADDR
配置静态地址
然后：
修改 /etc/udev/rules.d/70-persistent-net.rules
然后 reboot
<!--more-->
# linux命令继续学习
## 查看文件内容
cat    somefile    一次性将文件内容全部输出（控制台）
more   somefile     可以翻页查看, 下翻一页(空格)    上翻一页（b）   退出（q）
less   somefile      可以翻页查看,下翻一页(空格)    上翻一页（b），上翻一行(↑)  下翻一行（↓）  可以搜索关键字（/keyword）

tail -10  install.log   查看文件尾部的10行
tail -f install.log    小f跟踪文件的唯一inode号，就算文件改名后，还是跟踪原来这个inode表示的文件
tail -F install.log    大F按照文件名来跟踪

head -10  install.log   查看文件头部的10行

## 后台服务管理
service network status   查看指定服务的状态
service network stop     停止指定服务
service network start    启动指定服务
service network restart  重启指定服务
service --status-all  查看系统中所有的后台服务

## 设置后台服务的自启配置
chkconfig   查看所有服务器自启配置
chkconfig iptables off   关掉指定服务的自动启动
chkconfig iptables on   开启指定服务的自动启动

## 系统启动级别管理
vi  /etc/inittab
```
# Default runlevel. The runlevels used are:
#   0 - halt (Do NOT set initdefault to this)
#   1 - Single user mode
#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)
#   3 - Full multiuser mode
#   4 - unused
#   5 - X11
#   6 - reboot (Do NOT set initdefault to this)
#
id:3:initdefault:
```

# 软件安装



## 如何上传安装包到服务器

- 可以使用图形化工具，如： filezilla
- 可以使用sftp工具：  alt+p 调出后，用put命令上传
上传（如果不cd指定目录，则上传到当前用户的主目录）：
```
sftp> cd /home/   
sftp> put C:\Users\Administrator\Desktop\day02\soft\jdk-7u45-linux-x64.tar.gz
```
下载（lcd[local cd]指定下载到本地的目标路径）
```
sftp> lcd d:/                            
sftp> get /home/jdk-7u45-linux-x64.tar.gz
```

- lrzsz


## 安装jdk
### 压缩解压缩的相关命令
压缩解压缩

    root@mini1 ~]# gzip access.log 
    [root@mini1 ~]# ll
    总用量 134892
    -rw-r--r--. 1 root root        68 4月   3 17:37 access.log.gz

解压gz文件：  ```gzip -d access.log.gz```

### 打包解包
```
[root@mini1 ~]# tar -cvf myfirsttarball.tar aaa/
aaa/
aaa/2.txt
aaa/3.txt
aaa/1.txt
```
解包：
```
[root@mini1 ~]# tar -xvf myfirsttarball.tar 
aaa/
aaa/2.txt
aaa/3.txt
aaa/1.txt
```

### 一次性完成打包&&压缩的操作
产生压缩包：
```
[root@mini1 ~]# tar -zcvf my.tar.gz aaa/
aaa/
aaa/2.txt
aaa/3.txt
aaa/1.txt
```

解压缩包：
```
[root@mini1 ~]# tar -zxvf my.tar.gz 
aaa/
aaa/2.txt
aaa/3.txt
aaa/1.txt
```


### 安装jdk的过程：
- 解压安装包

    tar -zxvf jdk-7u45-linux-x64.tar.gz -C apps/
- 然后修改环境变量

    vi /etc/profile  
    在文件最后添加
    export JAVA_HOME=/root/apps/jdk1.7.0_45
    export PATH=$PATH:$JAVA_HOME/bin
    保存退出

- 然后重新加载环境变量 

    source /etc/profile



## 安装rpm包软件，如mysql
### 查看系统中安装的rpm包

    rpm -qa | grep mysql

### 上传rpm安装包

    MySQL-client-5.5.48-1.linux2.6.x86_64.rpm
    MySQL-server-5.5.48-1.linux2.6.x86_64.rpm
    per * .rpm

### 安装perl依赖

    rpm -ivh perl*
_可能会提示有包冲突，解决： rpm -e 冲突包名 --nodeps_

### 安装server

    rpm -ivh MySQL-server-5.5.48-1.linux2.6.x86_64.rpm
    
如果成功，会看到进度条，最后，有关于root密码设置的提示，一定要记下来
这个版本的提示是，先启动server

    service mysql start
    
然后```/usr/bin/mysql_secure_installation``` 命令去交互式修改root密码

### 修改密码时，提示需要先安装client

    rpm -ivh MySQL-client-5.5.48-1.linux2.6.x86_64.rpm

客户端安装成功后，记得还要用/usr/bin/mysql_secure_installation 命令去交互式修改root密码

### 登录验证

    mysql -uroot -proot