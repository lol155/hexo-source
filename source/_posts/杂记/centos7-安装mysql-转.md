---
title: centos7 安装mysql-转
categories: 学习
tags:
  - 大数据
  - linux
  - mysql
toc: true
date: 2017-11-04 22:30:14
scaffolds:
---
[linux下安装Mysql](http://www.cnblogs.com/xxoome/p/5864912.html)


# 1. 其他
## 1.1. 错误
### 1.1.1. 本地登陆不进去
```
#mysql -u root -p 
提示”Access denied for user ‘root’@’localhost’ (using password: YES)”
```
我的mysql版本 
> mysql  Ver 14.14 Distrib 5.6.38

先修改一下mysql安装目录下面my.cnf,最后一行添加 `skip-grant-tables`
然后登陆 进去修改
> update mysql.user set password=password(‘mypassword’) where user=’root’; 

退出,修改文件删除刚加的属性,重启mysql服务

### 1.1.2. 远程连接 ip无权限
> update mysql.user set host='%' where user=’root’;