---
title: linux下安装opencv-java 
categories: 学习
tags:
  - opencv
toc: true
date: 2018-01-24 15:03:02
scaffolds:
---
linux下安装opencv java 

# 1. 说明
在正式环境安装的时候安装opencv时候一直编译失败，缺少文件，后来下载了310版本的opencv，可以用了  
线上环境linux centos6.5

# 2. linux版本
centos7

# 3. 安装jdk
# 4. 安装ant
## 4.1. 下载
[ant官网下载](http://ant.apache.org/bindownload.cgi)http://ant.apache.org/bindownload.cgi
```
apache-ant-1.9.9-bin.tar.gz
```
## 4.2. 解压 重命名
```
unzip apache-ant-1.9.9-bin.tar.gz
mv apache-ant-1.9.9 ant
```

## 4.3. 配置环境变量
```
vim /etc/profile 
在文件后加入 
export ANT_HOME=/usr/local/ant 
export PATH=$ANT_HOME/bin:$PATH
```
## 4.4. 刷新环境变量
```
source /etc/profile
```

# 5. 安装OpenCV
## 5.1. 安装OpenCV依赖库
```
yum install -y build-essential gcc gcc-c++ cmake git pkgconfig gtk+-devel gtk2-devel python python-pip python-devel
```
## 5.2. 下载
可以到[官网](https://opencv.org/releases.html)下载 选择对应版本，我这里是3.3.1
https://opencv.org/releases.html
## 5.3. 解压
先解压,解压后进入目录,创建build目录,进入build目录,准备预编译
```
unzip opencv-3.3.1.zip
cd opencv-3.3.1 
mkdir build && cd build/
```
## 5.4. 预编译
通过cmake命令预先编译一次,编译完成后查看输出结果, 在```To be built```里包含java这一项就表示预编译成功
```
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF -DBUILD_EXAMPLES=OFF -DBUILD_TESTS=OFF -DBUILD_PERF_TESTS=OFF -DCMAKE_INSTALL_PREFIX=/usr/local ..
```
可能会卡在
```
-- IPPICV: Download: ippicv_2017u3_lnx_intel64_general_20170822.tgz
```
这个文件比较大,下载时间长一点,不要着急
## 5.5. 编译
cmake完成后，在build目录直接输入make进行编译，如果服务器CPU核数比较多，可以在make后加入-j8 使用8个线程同时进行编译，加快编译速度。
```
make -j2
```
关于下面这个异常，我编译的时候并没有出现，记录一下，备用。

编译过程可能出现异常，提示：

/usr/include/jasper/jas_math.h:117:22: error: ‘SIZE_MAX’ was not declared in this scope

出现这个异常，不要慌，可以通过修改/usr/include/jasper/jas_math.h 文件源码解决。 
在/usr/include/jasper/jas_math.h 的头部#include 的下面添加:
```
#if ! defined SIZE_MAX 
#define SIZE_MAX (4294967295U) 
#endif
```
然后重新编译就可以了。如果不行，请删除build目录下的内容，重新预编译、编译就应该没问题了
## 5.6. 安装
编译完成，就可以进行安装，安装过程是生成opencv对应的库文件，我这里是java项目需要用到，所以也会顺便生成java相关的库文件
```
make install
```
命令执行完成，会在结果中看到/usr/local/share/OpenCV/java/目录生成了两个java相关的依赖库文件 opencv-331.jar 和 libopencv_java331.so ， 当然，每个人的环境不同，可能目录也不同，具体看日志输出就行了。
```
[root@opencv java]# ll /usr/local/share/OpenCV/java
总用量 72012
-rwxr-xr-x 1 root root 73320721 11月  8 14:05 libopencv_java331.so
-rw-r--r-- 1 root root   414381 11月  8 10:03 opencv-331.jar
```
# 6. 参考链接
*  [opencv官方文档 Installation in Linux](https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html)
*  [CentOS6 - Linux下安装OpenCV](http://blog.csdn.net/chwshuang/article/details/78208273?locationNum=9&fps=1)

