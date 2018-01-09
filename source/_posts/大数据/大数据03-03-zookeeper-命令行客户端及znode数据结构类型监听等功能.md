---
title: 大数据03-03-zookeeper-命令行客户端及znode数据结构类型监听等功能
categories: 学习
tags:
  - 大数据
  - zookeeper
toc: true
date: 2017-11-19 23:54:06
scaffolds:
---

# 进入客户端
```
bin/zkCli.sh (连到本机)
connect zk2:2181（连到zk2机器上）

[zk: zk2:2181(CONNECTED) 3] 
```
# zookeeper结构
## 特性
1. Zookeeper：一个leader，多个follower组成的集群
1. 全局数据一致：每个server保存一份相同的数据副本，client无论连接到哪个server，数据都是一致的
1. 分布式读写，更新请求转发，由leader实施
1. 更新请求顺序进行，来自同一个client的更新请求按其发送顺序依次执行
1. 数据更新原子性，一次数据更新要么成功，要么失败
1. 实时性，在一定时间范围内，client能读到最新数据

<!-- more -->

## 数据结构 
1. 层次化的目录结构，命名符合常规文件系统规范(见下图)
1. 每个节点在zookeeper中叫做znode,并且其有一个唯一的路径标识
1. 节点Znode可以包含数据和子节点（但是EPHEMERAL类型的节点不能有子节点，下一页详细讲解）
1. 客户端应用可以在节点上设置监视器（后续详细讲解）	

## 数据结构图
![20171119232757](http://ovasdkxqr.bkt.clouddn.com/image/blog/20171119232757.png)

## 节点类型
1. Znode有两种类型：
    - 短暂（ephemeral）（断开连接自己删除）
    - 持久（persistent）（断开连接不删除）
1. Znode有四种形式的目录节点（默认是persistent ）
    - PERSISTENT
    - PERSISTENT_SEQUENTIAL（持久序列/test0000000019 ）
    - EPHEMERAL
    - EPHEMERAL_SEQUENTIAL
1. 创建znode时设置顺序标识，znode名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护
1. 在分布式系统中，顺序号可以被用于为所有的事件进行全局排序，这样客户端可以通过顺序号推断事件的顺序

# 命令行操作
> 运行 zkCli.sh –server <ip>进入命令行工具

```
    [zk: zk2:2181(CONNECTED) 2] help
    ZooKeeper -server host:port cmd args
            connect host:port
            get path [watch]
            ls path [watch]
            set path data [version]
            rmr path
            delquota [-n|-b] path
            quit 
            printwatches on|off
            create [-s] [-e] path data acl
            stat path [watch]
            close 
            ls2 path [watch]
            history 
            listquota path
            setAcl path acl
            getAcl path
            sync path
            redo cmdno
            addauth scheme auth
            delete path [version]
            setquota -n|-b val path
```
1. 使用 ls 命令来查看当前 ZooKeeper 中所包含的内容：
    ```
    [zk: zk2:2181(CONNECTED) 3] ls /
    [zookeeper]
    ```
1. 创建一个新的 znode ，使用 create /zk myData 。这个命令创建了一个新的 znode 节点“ zk ”以及与它**关联的**字符串：
    ```
    [zk: zk2:2181(CONNECTED) 4] create /app 1234
    Created /app
    ```

1. 我们运行 get 命令来确认 znode 是否包含我们所创建的字符串：
    ```
    [zk: zk2:2181(CONNECTED) 5] get /app
    1234
    cZxid = 0x300000004
    ctime = Sun Nov 19 15:48:32 UTC 2017
    mZxid = 0x300000004
    mtime = Sun Nov 19 15:48:32 UTC 2017
    pZxid = 0x300000004
    cversion = 0
    dataVersion = 0
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 4
    numChildren = 0

    ```

    ```
    [zk: localhost:2181(CONNECTED) 4] get /app watch
    #监听这个节点的变化,当另外一个客户端改变/app,它会打出下面的
    #WATCHER::
    #WatchedEvent state:SyncConnected type:NodeDataChanged path:/app
    ```

1. 下面我们通过 set 命令来对 zk 所关联的字符串进行设置：
    ```
    [zk: zk2:2181(CONNECTED) 6] set /app 222
    cZxid = 0x300000004
    ctime = Sun Nov 19 15:48:32 UTC 2017
    mZxid = 0x300000005
    mtime = Sun Nov 19 15:50:46 UTC 2017
    pZxid = 0x300000004
    cversion = 0
    dataVersion = 1
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 3
    numChildren = 0

    [zk: zk2:2181(CONNECTED) 7] get /app
    222
    cZxid = 0x300000004
    ctime = Sun Nov 19 15:48:32 UTC 2017
    mZxid = 0x300000005
    mtime = Sun Nov 19 15:50:46 UTC 2017
    pZxid = 0x300000004
    cversion = 0
    dataVersion = 1
    aclVersion = 0
    ephemeralOwner = 0x0
    dataLength = 3
    numChildren = 0
    ```
1. 下面我们将刚才创建的 znode 删除：只能删除没有子节点的节点
    ```
    [zk: zk2:2181(CONNECTED) 8] delete /app
    [zk: zk2:2181(CONNECTED) 9] ls /
    [zookeeper]
    ```
1. 删除节点：rmr 删除节点及其子节点
    ```
    [zk: zk2:2181(CONNECTED) 11] create /app2 12
    Created /app2
    [zk: zk2:2181(CONNECTED) 12] rmr /app2
    [zk: zk2:2181(CONNECTED) 13] ls /           
    [zookeeper]
    ```
