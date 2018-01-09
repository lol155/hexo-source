---
title: vagrant 创建virtualBox虚拟机
categories: 工具
toc: true
date: 2017-11-04 23:37:13
scaffolds:
tags: [vagrant]
---
# 安装vagrant
## 本机环境

系统环境 ： win10 64 系统

## 软件及镜像


vagrant 官网 https://www.vagrantup.com/  
vagrant 需要依赖虚拟机 我用的 virtualBox  
virtualBox 官网 https://www.virtualbox.org/   
virtualBox 镜像 在vagrant官网中可以下载  
镜像下载 http://www.vagrantbox.es/

由于国内下载比较慢，已分享到百度云
<!-- more -->

|名称|类型| 分享链接|
|:----|:----:|:---------:|
|vagrant_1.9.5.msi | 软件 | http://pan.baidu.com/s/1kVzFP1H|
|VirtualBox-5.1.22-115126-Win.exe | 软件 | http://pan.baidu.com/s/1jIQgl6m|
|centos-7.0-x86_64.box | 镜像文件 | http://pan.baidu.com/s/1o7Zlspk|
|gparted-live-0.28.1-1-i686.iso | 32位分区live-cd | http://pan.baidu.com/s/1jISiee6|
|gparted-live-0.28.1-1-amd64.iso | 64位分区live-cd | http://pan.baidu.com/s/1dFenvvb|

上面的gparted iso 文件是分区用的不过没有用到，下载不容易就一块传上来了。

## 更改vagrant 存放box的位置
默认vagrant 添加的box 默认存放位置为
> C:\Users\当前用户名\ .vagrant.d\boxes\ 

可以通过添加环境变量 VAGRANT_HOME 修改存放位置

例如：VAGRANT_HOME = D:\Program Files\VagrantHome

## 更改virtualBox 存放镜像目录
默认目录 

> C:\Users\当前用户名\VirtualBox VMs
  
可以在virtualBox 软件 > 管理 > 全局设置 > 常规 > 默认虚拟电脑位置进行修改

我修改为
> D:\VirtualBoxVMs
# 启动vagrant
## 添加box
> vagrant box add centos7 E:\软件安装包\centos-7.0-x86_64.box

查看已添加的box

> vagrant box list

    D:\vagrantSpace>vagrant box list
    centos7 (virtualbox, 0)

## 初始化

在准备存放vagrant配置文件的文件夹中执行

> vagrant init 

会生成 Vagrantfile 文件  
文件中包含vagrant 配置信息  
其中有一些配置项的说明  

    config.vm.box = "base" 修改base 为centos7 即 boxlist 中的名字  
    config.vm.synced_folder "E:\", "/vagrant_data" 共享文件夹
    
其他配置请自行百度

### vagrant 配置文件

下面的是我的配置

    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    Vagrant.configure("2") do |config|
      config.vm.box = "centos7"
      config.vm.define :java do |java|
        java.vm.provider "virtualbox" do |v|
            v.customize ["modifyvm", :id, "--name", "java", "--memory", "512"]
        end
        java.vm.box = "centos7"
        java.vm.hostname = "java"
        java.vm.network :private_network, ip: "192.168.33.10"
      end
      config.vm.define :linux do |linux|
        linux.vm.provider "virtualbox" do |v|
            v.customize ["modifyvm", :id, "--name", "linux", "--memory", "512"]
        end
        linux.vm.box = "centos7"
        linux.vm.hostname = "linux"
        linux.vm.network :private_network, ip: "192.168.33.11"
      end
    end

## 启动虚拟机

> vagrant up linux 

第一次会初始化虚拟机,并启动,成功后查看虚拟机运行状态

> vagrant global-status

    D:\vagrantSpace>vagrant global-status
    id       name   provider   state   directory
    -----------------------------------------------------------------------
    1d24610  linux  virtualbox running D:/vagrantSpace

这个时候就可以通过ssh登录虚拟机了。默认用户和密码都是 vagrant

------------------------------------
下面是修改磁盘大小,如果觉得没有必要,可以不修改,直接玩就可以啦

# 修改虚拟机磁盘大小

vagrant 默认创建的磁盘 根目录下只有10G.觉得太小,修改为1T.

- 关闭 linux

> vagrant halt linux

- 进入到 D:\VirtualBoxVMs文件夹下 即 之前设置的virtualBox的路径,里面会有个刚才创建的linux系统的文件夹 D:\VirtualBoxVMs\linux
- 加入virtualBox 安装目录加入环境变量path

    安装virtualBox后会自动加入 VBOX_MSI_INSTALL_PATH 环境变量,直接把这个路径加到path后面即可. 
    
> %VBOX_MSI_INSTALL_PATH%
    
- 通过下面命令 复制一份磁盘文件, .vmdk 文件直接修改大小会报错


> vboxmanage clonehd box-disk1.vmdk box.vdi --format vdi

    D:\VirtualBoxVMs\linux>vboxmanage clonehd box-disk1.vmdk box.vdi --format vdi
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
    Clone medium created in format 'vdi'. UUID: 1431c156-a3b1-4374-b196-36450edecd9e

- 修改vdi 文件大小,命令如下

> vboxmanage modifyhd box.vdi --resize 1048576

    D:\VirtualBoxVMs\linux>vboxmanage modifyhd box.vdi --resize 1048576
    0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%

- 打开虚拟机,修改 linux 磁盘文件为 box.vdi

![](https://static.oschina.net/uploads/img/201707/02192036_F9Jv.jpg)

修改后

![](https://static.oschina.net/uploads/img/201707/02192410_T07t.jpg)

- 查看D:\VirtualBoxVMs\linux 文件夹下linux.vbox 文件 
删除 原来的harddisk 
```
     <HardDisk uuid="{3417d3e1-fcfd-410c-8df6-adba5f8b01bb}" location="box-disk1.vmdk" format="VMDK" type="Normal"/>
```

保留新的harddisk
```
    <HardDisk uuid="{417759f6-4e5d-4a45-9ce1-1351b15c5a7d}" location="box-linux.vdi" format="VDI" type="Normal"/>
```


- 删除原来的磁盘文件, box-disk1.vmdk。这个可以以后再删除也可以，防止操作中出现什么错误。
- 启动虚拟机
> vagrant up linux

切换到root，密码为vagrant

> su

查看磁盘

> fdisk -l

    Gerät  boot.     Anfang        Ende     Blöcke   Id  System
    /dev/sda1   *        2048     1026047      512000   83  Linux
    /dev/sda2         1026048    20766719     9870336   8e  Linux LVM

可以看到一共还是10G,并不是1T,我们需要加一个磁盘sda3

    fdisk /dev/sda
    n
    p
    3
    回车
    回车
更改磁盘类型为 lvm

    t
    3
    8e


查看磁盘

    p
全部操作如下
    
    Befehl (m für Hilfe): n
    Partition type:
       p   primary (2 primary, 0 extended, 2 free)
       e   Erweiterte
    Select (default p): p
    Partitionsnummer (3,4, default 3): 3
    Erster Sektor (20766720-2147483647, Vorgabe: 20766720): 
    Benutze den Standardwert 20766720
    Last Sektor, +Sektoren or +size{K,M,G} (20766720-2147483647, Vorgabe: 2147483647): 
    Benutze den Standardwert 2147483647
    Partition 3 of type Linux and of size 1014,1 GiB is set
    
    Befehl (m für Hilfe): t
    Partitionsnummer (1-3, default 3): 3
    Hex code (type L to list all codes): 8e
    Changed type of partition 'Linux' to 'Linux LVM'
    
    Befehl (m für Hilfe): p
    
    Disk /dev/sda: 1099.5 GB, 1099511627776 bytes, 2147483648 sectors
    Units = Sektoren of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x00095a8e
    
       Gerät  boot.     Anfang        Ende     Blöcke   Id  System
    /dev/sda1   *        2048     1026047      512000   83  Linux
    /dev/sda2         1026048    20766719     9870336   8e  Linux LVM
    /dev/sda3        20766720  2147483647  1063358464   8e  Linux LVM

保存退出

    w
重启虚拟机
    
    reboot
查看当前 Volume Group

    vgdisplay
信息如下

    [root@linux vagrant]# vgdisplay
      --- Volume group ---
      VG Name               centos
      System ID             
      Format                lvm2
      Metadata Areas        1
      Metadata Sequence No  3
      VG Access             read/write
      VG Status             resizable
      MAX LV                0
      Cur LV                2
      Open LV               2
      Max PV                0
      Cur PV                1
      Act PV                1
      VG Size               9,41 GiB
      PE Size               4,00 MiB
      Total PE              2409
      Alloc PE / Size       2399 / 9,37 GiB
      Free  PE / Size       10 / 40,00 MiB
      VG UUID               NJWfOH-An0K-Hm9Q-8Tjs-yq2x-ZWXi-L2vP7i
      
名称为 centos，可调整大小（resizable），当前大小为 9,41GB。活动的 LVM 卷有

    lvscan
    
    [root@linux vagrant]# lvscan
    ACTIVE            '/dev/centos/swap' [1016,00 MiB] inherit
    ACTIVE            '/dev/centos/root' [8,38 GiB] inherit
    
先将分配过来的新磁盘空间创建为一个新的物理卷

    pvcreate /dev/sda3
    
    [root@linux vagrant]# pvcreate /dev/sda3
    Physical volume "/dev/sda3" successfully created
    
然后使用新的物理卷来扩展 LVM 的 centos

    vgextend centos /dev/sda3
    
    [root@linux vagrant]# vgextend centos /dev/sda3
    Volume group "centos" successfully extended

然后扩展 LVM 的逻辑卷 centos-lv_root

    lvextend /dev/centos/root /dev/sda3
    
    [root@linux vagrant]# lvextend /dev/centos/root /dev/sda3
    Size of logical volume centos/root changed from 8,38 GiB (2145 extents) to 1022,47 GiB (261753 extents).
    Logical volume root successfully resized
    
最后，调整逻辑卷文件系统的大小

    vagrant]# resize2fs /dev/centos/root
    
    [root@linux vagrant]# resize2fs /dev/centos/root
    resize2fs 1.42.9 (28-Dec-2013)
    resize2fs: Ungültige magische Zahl im Superblock beim Versuch, /dev/centos/root zu öffnen
    Kann keinen gültigen Dateisystem-Superblock finden.
    
完成。看看效果,可以看到root下变成了1T

    lvscan
    
    [root@linux vagrant]# lvscan
      ACTIVE            '/dev/centos/swap' [1016,00 MiB] inherit
      ACTIVE            '/dev/centos/root' [1022,47 GiB] inherit
      
但是,查看根目录下面分配大小, 跟目录还是为8.4G

    [root@linux vagrant]# df -h
    Dateisystem             Größe Benutzt Verf. Verw% Eingehängt auf
    /dev/mapper/centos-root  8,4G    1,1G  7,4G   13% /
    devtmpfs                 236M       0  236M    0% /dev
    tmpfs                    245M       0  245M    0% /dev/shm
    tmpfs                    245M    4,3M  241M    2% /run
    tmpfs                    245M       0  245M    0% /sys/fs/cgroup
    /dev/sda1                497M    118M  379M   24% /boot  
 
用 xfs_growfs 对扩容后的 LV 进行 xfs 格式大小调整

    xfs_growfs /dev/centos/root
    
    [root@linux vagrant]# xfs_growfs /dev/centos/root
    meta-data=/dev/mapper/centos-root isize=256    agcount=4, agsize=549120 blks
             =                       sectsz=512   attr=2, projid32bit=1
             =                       crc=0        finobt=0
    data     =                       bsize=4096   blocks=2196480, imaxpct=25
             =                       sunit=0      swidth=0 blks
    naming   =version 2              bsize=4096   ascii-ci=0 ftype=0
    log      =Intern                 bsize=4096   blocks=2560, version=2
             =                       sectsz=512   sunit=0 blks, lazy-count=1
    realtime =keine                  extsz=4096   blocks=0, rtextents=0
    Datenblöcke von 2196480 auf 268035072 geändert.

再查看

    [root@linux vagrant]# df -h
    Dateisystem             Größe Benutzt Verf. Verw% Eingehängt auf
    /dev/mapper/centos-root 1023G    1,1G 1022G    1% /
    devtmpfs                 236M       0  236M    0% /dev
    tmpfs                    245M       0  245M    0% /dev/shm
    tmpfs                    245M    4,3M  241M    2% /run
    tmpfs                    245M       0  245M    0% /sys/fs/cgroup
    /dev/sda1                497M    118M  379M   24% /boot
    
可以看到根目录大小已经变成1T了

http://pan.baidu.com/s/1dEC3ePr

由于不了解linux,修改磁盘大小费了很多事.完成后打了包,做成了新的box，方便以后直接使用。

# 其他
## 修改linux 语言
> /etc/locale.conf
```
LANG='en_US.UTF-8'
```


# 参考博客:  

## 调整 VirtualBox 虚拟机的磁盘大小  
https://cnzhx.net/blog/resizing-lvm-centos-virtualbox-guest/  

## 手把手教你给 CentOS 7 添加硬盘及扩容(LVM)  
https://aurthurxlc.github.io/Aurthur-2017/Centos-7-extend-lvm-volume.html
