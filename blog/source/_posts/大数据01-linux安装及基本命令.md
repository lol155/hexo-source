---
title: 大数据01-linux安装及基本命令.md
categories: 大数据学习笔记
tags: [linux,大数据,linux命令]
toc: true
date: 2017-10-30 22:30:38
scaffolds:
---

# linux 文件目录
> 约定俗成

目录 | 功能
------- | -------
/bin | 可执行文件
/sbin | 系统可执行文件
/boot | 跟启动相关的东西
/dev | 设备 硬件
/etc | 配置文件 
/lib | 依赖包
/lib64 | 依赖包
/media | 一些外接设备
/mnt | 挂载
/home | 用户目录
/usr | user share resource 公用的一些软件
/tmp | 临时文件



<!-- more -->
# linux 网络相关
## 网卡相关:
1. ip ：一般 1 给网关  255是广播地址
2. gateway ： 网关
3. netmask ： 子网掩码
4. dns ：

### 子网掩码
是用来判断自己属于哪个网段

网段的计算:
将IP地址的二进制 & 子网掩码的二进制 = 网段地址
例如：192.168.33.2 & 255.255.255.0 = 192.168.33.0
### dns 域名解析
1. 从本地hosts中寻找域名 -> ip映射信息
2. 如果没有则去服务器找
3. 访问服务器

填写:
1. 网关地址
2. dns服务器

![域名工作流程](http://ovasdkxqr.bkt.clouddn.com/dashuju/%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E7%9A%84%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B.png)

linux dns配置文件
> /etc/hosts

## 网络模式
### NAT模式  
![NAT模式图](http://ovasdkxqr.bkt.clouddn.com/dashuju/NAT虚拟网络配置.png)  
### 桥接模式  
![桥接模式图](http://ovasdkxqr.bkt.clouddn.com/dashuju/ssh%E5%85%8D%E5%AF%86%E7%99%BB%E9%99%86%E6%9C%BA%E5%88%B6%E7%A4%BA%E6%84%8F%E5%9B%BE.png)
### HOST only  
![host only](http://ovasdkxqr.bkt.clouddn.com/dashuju/%E6%A1%A5%E6%8E%A5%E5%92%8Chostonly%E8%99%9A%E6%8B%9F%E7%BD%91%E7%BB%9C%E9%85%8D%E7%BD%AE.png)

## 修改linux ip
修改主机名:  
> vi /etc/sysconfig/network  
    NETWORKING=yes
    HOSTNAME=server1.itcast.cn

修改IP地址  
vi /etc/sysconfig/network-scripts/ifcfg-eth0  

    DEVICE=eth0
    TYPE=Ethernet
    ONBOOT=yes          #是否开机启用
    BOOTPROTO=static    #ip地址设置为静态
    IPADDR=192.168.0.101
    NETMASK=255.255.255.0

修改ip地址和主机名的映射关系  
vi /etc/hosts  

    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    192.168.0.101 server1.itcast.cn

关闭iptables并设置其开机启动/不启动  

    service iptables stop
    chkconfig iptables on
    chkconfig iptables off

# linux 简单命令
创建文件
touch somefile.1 创建一个空文本
echo "i love u" **>** somefile.2 利用重定向写入 覆盖原文件
echo "i love u" **>>** somefile.2 追加

ctrl+v进入块选择模式
shift+v 进入行选择模式

:%s/key/newword 查找并替换

## 文件权限操作
drw-r--r--

d：标识节点类型（d 文件夹 - 文件 l 链接)
r 可读 w可写 x可执行
拥有者 所属组 其他用户

方式1 :
chmod g-rw somefile.1

g 组 o 其他 u用户  +-(增加/删除权限)  rwx

方式2 : 
chmod 700 somfile.1

文件夹及下面文件

chmod -R 770 aaa/

修改所有者(必须要root用户)
chown angela:angela aaa/   组/用户

## 基本的用户管理
添加用户

useradd nagela
修改密码
passwd nagela 按提示输入密码即可

sudo useradd xiaobai

为用户配置sudo权限
用root编辑 
vi /etc/sudoers 
在文件如下位置,为hadoop添加一行即可
root ALL=(ALL) ALL  
hadoop ALL=(ALL) ALL  

## 系统管理操作
1.查看主机名
hostname

2.修改主机名(重启后无效)
hostname hadoop

3.修改主机名(重启后永久生效)
vi /ect/sysconfig/network

4.修改IP(重启后无效)
ifconfig eth0 192.168.12.22

5.修改IP(重启后永久生效)
vi /etc/sysconfig/network-scripts/ifcfg-eth0

6.查看系统信息
uname -a
uname -r

7.查看ID命令
id -u
id -g

8.日期
date
date +%Y-%m-%d
date +%T
date +%Y-%m-%d" "%T

9.日历
cal 2012

10.查看文件信息
file filename

11.挂载硬盘
mount
umount
加载windows共享
mount -t cifs //192.168.1.100/tools /mnt

12.查看文件大小
du -h
du -ah

13.查看分区
df -h

14.ssh
ssh hadoop@192.168.1.1

15.关机
shutdown -h now /init 0
shutdown -r now /reboot


# ssh免密登录
![](http://ovasdkxqr.bkt.clouddn.com/dashuju/ssh%E5%85%8D%E5%AF%86%E7%99%BB%E9%99%86%E6%9C%BA%E5%88%B6%E7%A4%BA%E6%84%8F%E5%9B%BE.png)

假如 A  要登陆  B
在A上操作：
首先生成密钥对

    ssh-keygen   (提示时，直接回车即可)
再将A自己的公钥拷贝并追加到B的授权列表文件authorized_keys中

    ssh-copy-id   B



