---
title: 大数据02-03 shell编程语法讲解
categories: 学习
tags:
  - 大数据
  - linux
  - centos7
  - linux命令
toc: true
date: 2017-11-06 23:02:36
scaffolds:
---

# 变量
## 系统变量
* $HOME
* $PWD
* $SHELL
* $USER
显示当前shell中所有变量
```
set
```
<!-- more -->
## 用户自定义变量
定义变量
> STR="hello world"

> A=9

> unset A 撤销变量 A

> readonly B=2 声明静态的变量 B=2 ，不能 unset

> export 变量名 可把变量提升为全局环境变量，可供其他shell程序使用

将命令的返回值赋给变量

```
A=`ls -la` 反引号，运行里面的命令，并把结果返回给变量A
A=$(ls -la) 等价于反引号
```
## shell中的特殊变量
* $? 表示上一个命令退出的状态  true 0 false 1 错误127
* $$ 表示当前进程编号
* $0 表示当前脚本名称
* $n 表示n位置的输入参数（n代表数字，n>=1）
* $# 	表示参数的个数，常用于循环
* $*和$@ 都表示参数列表 
### $*与$@区别
* $* 和 $@ 都表示传递给函数或脚本的所有参数，不被双引号" "包含时，都以$1  $2  … $n 的形式输出所有参数  
* 当它们被双引号" "包含时，"$*" 会将所有的参数作为一个整体，以"$1 $2 … $n"的形式输出所有参数；"$@" 会将各个参数分开，以"$1" "$2" … "$n" 的形式输出所有参数

## 运算符
> 格式 :expr m + n 或$((m+n)) 注意expr运算符间要有空格

例如计算（2 ＋3 ）×4 的值
1 .分步计算	S=`expr 2 + 3`	expr $S \* 4
2.一步完成计算
```bash
	expr `expr 2 + 3 ` \* 4
	echo `expr \`expr 2 + 3\` \* 4`
	或
	$(((2+3)*4))
```
# for循环

## 第一种：
```bash
for N in 1 2 3
do
	echo $N
done
或
for N in 1 2 3; do echo $N; done
或
for N in {1..3}; do echo $N; done
```
## 第二种：
```bash
for ((i = 0; i <= 5; i++))
do
	echo "welcome $i times"
done
或
for ((i = 0; i <= 5; i++)); do echo "welcome $i times"; done
```

# while循环

## 第一种
```bash
while expression
do
command
…
done
```

## 第二种
```bash
i=1
while ((i<=3))
do
  echo $i
  let i++
done
```
# case语句

格式
```bash
case $1 in
start)
	echo "starting"
	;;
stop)
	echo "stoping"
	;;
*)
	echo "Usage: {start|stop} “
esac

```
# read命令
read -p(提示语句)-n(字符个数) -t(等待时间) 
	read -p "please input your name: " NAME

使用示例：
```
[vagrant@mysql1 ~]$ read -p "please enter a number:" numb
please enter a number:1
[vagrant@mysql1 ~]$ echo $numb
1
[vagrant@mysql1 ~]$ 
```

# if判断
语法
```bash
if condition 
then 
    statements
[elif condition 
    then statements. ..] 
[else 
    statements ] 
fi

```
# 判断语句
[ condition ] **(注意condition前后要有空格)**
#非空返回true，可使用$?验证（0为true，>1为false）
[ test ]

#空返回false
[  ]

> [ condition ] && echo OK || echo notok

条件满足，执行后面的语句 不满足执行 || 后面的语句

# 常用判断条件
* = 字符串比较
* -lt 小于
* -le 小于等于
* -eq 等于
* -gt 大于
* -ge 大于等于
* -ne 不等于

* -r 有读的权限
* -w 有写的权限
* -x 有执行的权限
* **-f 文件存在并且是一个常规的文件**
* -s 文件存在且不为空
* -d 文件存在并是一个目录
* -b文件存在并且是一个块设备
* -L 文件存在并且是一个链接

# Shell自定义函数
语法
```bash
 [ function ] funname [()]
{
    action;
    [return int;]
}

```
三种声明方式:
* function start()  
* function start 
* start()

注意
1. 必须在调用函数地方之前，先声明函数，shell脚本是逐行运行。不会像其它语言一样先预编译
2. 函数返回值，只能通过$? 系统变量获得，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)

## 例子
```bash
#!/bin/bash
fSum 3 2;
function fSum()
{
    echo $1,$2;
    return $(($1+$2));
}
fSum 5 7;
total=$?;
echo $total,$?;

```
结果
```
[vagrant@mysql1 ~]$ sh test 
test: line 2: fSum: command not found
5,7
12,0

```
# 脚本调试
* sh -vx helloWorld.sh
* 或者在脚本中增加set -x

# cut命令
cut命令可以从一个文本文件或者文本流中提取文本列。

## 语法
```
cut -d'分隔字符' -f fields <==用于有特定分隔字符
cut -c 字符区间            <==用于排列整齐的信息

```
* -d  ：后面接分隔字符。与 -f 一起使用；
* -f  ：依据 -d 的分隔字符将一段信息分割成为数段，用 -f 取出第几段的意思；
* -c  ：以字符 (characters) 的单位取出固定字符区间；

## 例子
### PATH 变量如下

```
[root@www ~]# echo $PATH
/bin:/usr/bin:/sbin:/usr/sbin:/usr/local/bin:/usr/X11R6/bin:/usr/games
# 1 | 2       | 3   | 4       | 5            | 6            | 7

```
### 将 PATH 变量取出，我要找出第五个路径。

```
#echo $PATH | cut -d ':' -f 5
/usr/local/bin

```
### 将 PATH 变量取出，我要找出第三和第五个路径。

```
#echo $PATH | cut -d ':' -f 3,5
/sbin:/usr/local/bin

```
### 将 PATH 变量取出，我要找出第三到最后一个路径。

```
echo $PATH | cut -d ':' -f 3-
/sbin:/usr/sbin:/usr/local/bin:/usr/X11R6/bin:/usr/games

```

### 将 PATH 变量取出，我要找出第一到第三个路径。

```
#echo $PATH | cut -d ':' -f 1-3
/bin:/usr/bin:/sbin:

```
### 将 PATH 变量取出，我要找出第一到第三，还有第五个路径。

```
echo $PATH | cut -d ':' -f 1-3,5
/bin:/usr/bin:/sbin:/usr/local/bin
```

## 实用例子:只显示/etc/passwd的用户和shell

```
#cat /etc/passwd | cut -d ':' -f 1,7 
root:/bin/bash
daemon:/bin/sh
bin:/bin/sh
```

# sort命令
sort 命令对 File 参数指定的文件中的行排序，并将结果写到标准输出。如果 File 参数指定多个文件，那么 sort 命令将这些文件连接起来，并当作一个文件进行排序。
## 语法
`[root@www ~]# sort [-fbMnrtuk] [file or stdin]`
选项与参数：
* -f  ：忽略大小写的差异，例如 A 与 a 视为编码相同；
* -b  ：忽略最前面的空格符部分；
* -M  ：以月份的名字来排序，例如 JAN, DEC 等等的排序方法；
* -n  ：使用『纯数字』进行排序(默认是以文字型态来排序的)；
* -r  ：反向排序；
* -u  ：就是 uniq ，相同的数据中，仅出现一行代表；
* -t  ：分隔符，默认是用 [tab] 键来分隔；
* -k  ：以那个区间 (field) 来进行排序的意思

## 对/etc/passwd 的账号进行排序
```bash
[root@www ~]# cat /etc/passwd | sort
adm:x:3:4:adm:/var/adm:/sbin/nologin
apache:x:48:48:Apache:/var/www:/sbin/nologin
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin

```
sort 是默认以第一个数据来排序，而且默认是以字符串形式来排序,所以由字母 a 开始升序排序。
## /etc/passwd 内容是以 : 来分隔的，我想以第三栏来排序，该如何

```
[root@www ~]# cat /etc/passwd | sort -t ':' -k 3
root:x:0:0:root:/root:/bin/bash
uucp:x:10:14:uucp:/var/spool/uucp:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
bin:x:1:1:bin:/bin:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin

```
## 默认是以字符串来排序的，如果想要使用数字排序：
```
cat /etc/passwd | sort -t ':' -k 3n
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh

```
## 默认是升序排序，如果要倒序排序，如下

```
cat /etc/passwd | sort -t ':' -k 3nr
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
ntp:x:106:113::/home/ntp:/bin/false
messagebus:x:105:109::/var/run/dbus:/bin/false
sshd:x:104:65534::/var/run/sshd:/usr/sbin/nologin

```
## 如果要对/etc/passwd,先以第六个域的第2个字符到第4个字符进行正向排序，再基于第一个域进行反向排序。

```
cat /etc/passwd |  sort -t':' -k 6.2,6.4 -k 1r      
sync:x:4:65534:sync:/bin:/bin/sync
proxy:x:13:13:proxy:/bin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh

```
## 查看/etc/passwd有多少个shell:对/etc/passwd的第七个域进行排序，然后去重:
```
cat /etc/passwd |  sort -t':' -k 7 -u
root:x:0:0:root:/root:/bin/bash
syslog:x:101:102::/home/syslog:/bin/false
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
sshd:x:104:65534::/var/run/sshd:/usr/sbin/nologin
```
# uniq去重命令
 uniq命令可以去除排序过的文件中的重复行，因此uniq经常和sort合用。也就是说，为了使uniq起作用，所有的重复行必须是相邻的。

## uniq语法
`[root@www ~]# uniq [-icu]`
选项与参数：
* -i   ：忽略大小写字符的不同；
* -c  ：进行计数
* -u  ：只显示唯一的行

testfile的内容如下
```
cat testfile
hello
world
friend
hello
world
hello
```
直接删除未经排序的文件，将会发现没有任何行被删除
```
#uniq testfile  
hello
world
friend
hello
world
hello
```
排序文件，默认是去重
```
#cat testfile | sort |uniq
friend
hello
world
```
排序之后删除了重复行，同时在行首位置输出该行重复的次数
```
#sort testfile | uniq -c
1 friend
3 hello
2 world
```
仅显示存在重复的行，并在行首显示该行重复的次数
```
#sort testfile | uniq -dc
3 hello
2 world
```
仅显示不重复的行
```
sort testfile | uniq -u
friend  
```


# wc命令
## 语法
`[root@www ~]# wc [-lwm]`
选项与参数：
* -l  ：仅列出行；
* -w  ：仅列出多少字(英文单字)；
* -m  ：多少字符；
## 默认使用wc统计/etc/passwd

```
#wc /etc/passwd
40   45 1719 /etc/passwd
40是行数，45是单词数，1719是字节数

```
wc的命令比较简单使用，每个参数使用如下：
 
```
#wc -l /etc/passwd   #统计行数，在对记录数时，很常用
40 /etc/passwd       #表示系统有40个账户

#wc -w /etc/passwd  #统计单词出现次数
45 /etc/passwd

#wc -m /etc/passwd  #统计文件的字符数
1719

```








