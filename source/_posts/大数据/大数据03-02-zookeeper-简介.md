---
title: 大数据03-02-zookeeper 简介
categories: 学习
tags:
  - 大数据
  - zookeeper
toc: true
date: 2017-11-19 20:22:13
scaffolds:
---

# 学软件，框架
需要明白三点
1. 应用场景：干什么的 在哪里用的
2. 核心工作机制流程功能：组成，角色
3. 怎么用
4. 再深一层，原理细节：选择性的深入了解，重要框架
<!--more-->

# zookeeper
zookeeper会用
# 概念简介
分布式**协调服务**，
- zookeeper是为别的分布式程序服务的
- Zookeeper**本身就是一个分布式程序**（只要有半数以上节点存活，zk就能正常服务）
- Zookeeper所提供的服务涵盖：主从协调、服务器节点动态上下线、统一配置管理、分布式共享锁、统一名称服务……
- 虽然说可以提供各种服务，但是zookeeper在底层其实只提供了两个功能：
    - **管理(存储，读取)用户程序提交的数据；**
    - **并为用户程序提供数据节点监听服务；**


## zookeeper一些应用场景
![20171118194439](http://ovasdkxqr.bkt.clouddn.com/image/blog/20171118194439.png)

## 集群角色分配原理
![20171118201716](http://ovasdkxqr.bkt.clouddn.com/image/blog/20171118201716.png)

## 集群机制
半数机制：集群中半数以上机器存活，集群可用。  
zookeeper适合装在奇数台机器上！！！
## 安装
### 上传文件
```
zookeeper-3.4.5.tar.gz
```
### 解压
```
tar -zxvf zookeeper-3.4.5.tar.gz
```
### 重命名
```
mv zookeeper-3.4.5 zookeeper（重命名文件夹zookeeper-3.4.5为zookeeper）
```
### 修改环境变量
1.  su root (切换到root用户)
2.  vi /etc/profile (修改文件)
3.  添加内容：
    ```
    export ZOOKEEPER_HOME=/home/hadoop/zookeeper
    export PATH=$PATH:$ZOOKEEPER_HOME/bin
    ```
4.  重新编译文件：
source /etc/profile
5.  注意：3台zookeeper都需要修改
6.  修改完成后切换回hadoop用户：
    ```
    su - hadoop
    ```
### 修改配置文件
1. 用hadoop用户操作
    ```
    cd zookeeper/conf
    cp zoo_sample.cfg zoo.cfg
    ```
2. vi zoo.cfg
3. 添加内容：
    ```
    dataDir=/home/hadoop/zookeeper/data
    dataLogDir=/home/hadoop/zookeeper/log
    server.1=zk1:2888:3888 (主机名, 心跳端口、数据端口)
    server.2=zk2:2888:3888
    server.3=zk3:2888:3888
    ```
### 创建文件夹
```
cd /home/hadoop/zookeeper/
mkdir -m 755 data
mkdir -m 755 log  
```
### 在data文件夹下新建myid文件，myid的文件内容为：
cd data
vi myid
添加内容：（数字递增）
```
1
```
上面的操作都是所有机器一起操作的，如果是单台操作需要把内容复制到其他的机器上
#### 将集群下发到其他机器上
```
scp -r /home/hadoop/zookeeper hadoop@slave2:/home/hadoop/
scp -r /home/hadoop/zookeeper hadoop@slave3:/home/hadoop/
```
#### 修改其他机器的配置文件
```
到slave2上：修改myid为：2
到slave3上：修改myid为：3
```
### 启动每台机器
```
/root/zookeeper/bin/zkServer.sh start
```
### 查看集群状态
- jps（查看进程）
- zkServer.sh status（查看集群状态，主从信息）


# 其他 
## 一般集群公司内部使用的时候防火墙是关掉了，不会被外界访问
```
service iptables stop
chkconfig iptables off
```

## 遇到错误启动不了
查看bin下的 zookeeper.out
## 注意
conf/zoo.cfg中节点ID必须与myid文件中的id相对应