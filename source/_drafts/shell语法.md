---
title: shell语法
tags:
---
# 1. 字符串操作
* 单引号 中变量、转义字符不能使用 以字符串输出
* 双引号 用可以使用变量、转义字符
* 字符串长度 `${{'{#'}}string}`  string 为变量名
* 字符串截取 `${string:1:4}`
* 查找字符串 `echo expr index "$string" io`  其中开头结尾是反引号 不是单引号

# 2. 数组
支持一维数组,不支持多维数组
* 定义数组
    * 数组名=(值1 值2 ... 值n)
    ```
        array_name=(value0 value1 value2 value3)
        或
        array_name=(
            value0
            value1
            value2
            value3
            )
        或
        array_name[0]=value0
        array_name[1]=value1
        array_name[n]=valuen
    ```
* 读取
    * ${数组名[下标]}
    ```
    获取下标为n的元素
    valuen=${array_name[n]}
    获取所有元素
    echo ${array_name[@]}
    ```
* 获取数组个数
```
# 取得数组元素的个数
length=${{'{#'}}array_name[@]}
# 或者
length=${{'{#'}}array_name[*]}
# 取得数组单个元素的长度
lengthn=${{'{#'}}array_name[n]}
```
# 3. 注释
* #开头
* 用花括号括起,定义成函数,不被调用不会执行,达到注释的效果
* 多行注释
```
:<<EOF
注释内容...
注释内容...
注释内容...
EOF

:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```

# 4. 传参
$0 $1 $2
* $#	传递到脚本的参数个数
* $*	以一个单字符串显示所有向脚本传递的参数。如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
* $@	与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
* $$	脚本运行的当前进程ID号
* $!	后台运行的最后一个进程的ID号
* $-	显示Shell使用的当前选项，与set命令功能相同。
* $?	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

> $* 与 $@ 区别：
```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "-- \$* 演示 ---"
for i in "$*"; do
    echo $i
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
    echo $i
done
```
结果
```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```
> 判断第n个参数是否存在
```
if [ -n "$1" ]; then
    echo "包含第一个参数"
else
    echo "没有包含第一参数"
fi
```
`中括号 [] 与其中间的代码应该有空格隔开`

# 5. 运算符
## 5.1. 包括
1. 算数运算符
2. 关系运算符
3. 布尔运算符
4. 字符串运算符
5. 文件测试运算符
> 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
> expr 是一款表达式计算工具，使用它能完成表达式的求值操作。
> 例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ')：
```
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```
> 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
> 完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

## 5.2. 算术运算符
下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：
运算符	 | 说明	 | 举例
--|----|-----------------------
+ | 加法 | \`expr $a + $b\` 结果为 30。
- | 减法 | \`expr $a - $b\` 结果为 -10。
* | 乘法 | \`expr $a \\* $b\` 结果为 200。注意*号前有\\
/ | 除法 | \`expr $b / $a\` 结果为 2。
% | 取余 | \`expr $b % $a\` 结果为 0。
= | 赋值 | a=$b 将把变量 b 的值赋给 a。
== | 相等。用于比较两个数字，相同则返回 true。 | [ $a == $b ] 返回 false。
!= | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。
> 条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。
## 5.3. 关系运算符
> 关系运算符只支持数字，不支持字符串，除非字符串的值是数字。
> 下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

运算符 | 说明 | 举例
----|----|---
-eq | 检测两个数是否相等，相等返回 true。 | [ $a -eq $b ] 返回 false。
-ne | 检测两个数是否不相等，不相等返回 true。 | [ $a -ne $b ] 返回 true。
-gt | 检测左边的数是否大于右边的，如果是，则返回 true。 | [ $a -gt $b ] 返回 false。
-lt | 检测左边的数是否小于右边的，如果是，则返回 true。 | [ $a -lt $b ] 返回 true。
-ge | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。
-le | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。

## 5.4. 布尔运算符
> 下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

运算符 | 说明 | 举例
----|----|---
! | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。
-o | 或运算，有一个表达式为 true 则返回 true。 | [ $a -lt 20 -o $b -gt 100 ] 返回 true。
-a | 与运算，两个表达式都为 true 才返回 true。 | [ $a -lt 20 -a $b -gt 100 ] 返回 false。

## 5.5. 逻辑运算符
> 以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

运算符 | 说明 | 举例
----|----|---
&& | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false
\|\| | 逻辑的 OR | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true

## 5.6. 字符运算符
> 下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

运算符 | 说明 | 举例
----|----|---
= | 检测两个字符串是否相等，相等返回 true。 | [ $a = $b ] 返回 false。
!= | 检测两个字符串是否相等，不相等返回 true。 | [ $a != $b ] 返回 true。
-z | 检测字符串长度是否为0，为0返回 true。 | [ -z $a ] 返回 false。
-n | 检测字符串长度是否为0，不为0返回 true。 | [ -n "$a" ] 返回 true。
str | 检测字符串是否为空，不为空返回 true。 | [ $a ] 返回 true。

true 和false 打印不出 可以作为条件判断

## 5.7. 文件测试运算符
> 文件测试运算符用于检测 Unix 文件的各种属性。

操作符 | 说明 | 举例
----|----|---
-b file | 检测文件是否是块设备文件，如果是，则返回 true。 | [ -b $file ] 返回 false。
-c file | 检测文件是否是字符设备文件，如果是，则返回 true。 | [ -c $file ] 返回 false。
-d file | 检测文件是否是目录，如果是，则返回 true。 | [ -d $file ] 返回 false。
-f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。
-g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。 | [ -g $file ] 返回 false。
-k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。 | [ -k $file ] 返回 false。
-p file | 检测文件是否是有名管道，如果是，则返回 true。 | [ -p $file ] 返回 false。
-u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。 | [ -u $file ] 返回 false。
-r file | 检测文件是否可读，如果是，则返回 true。 | [ -r $file ] 返回 true。
-w file | 检测文件是否可写，如果是，则返回 true。 | [ -w $file ] 返回 true。
-x file | 检测文件是否可执行，如果是，则返回 true。 | [ -x $file ] 返回 true。
-s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。 | [ -s $file ] 返回 true。
-e file | 检测文件（包括目录）是否存在，如果是，则返回 true。 | [ -e $file ] 返回 true。

# 6. echo命令
* read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量
    * -p 输入提示文字
    * -n 输入字符长度限制(达到6位，自动结束)
    * -t 输入限时
    * -s 隐藏输入内容
* echo -e "OK! \n" # -e 开启转义
* echo -e "OK! \c" # -e 开启转义 \c 不换行
* echo "It is a test" > myfile
* echo '$name\"'    原样输出字符串，不进行转义或取变量(用单引号)
* echo \`date\` 显示命令执行结果 `这里使用的是反引号 , 而不是单引号 '`

# 7. printf
> printf  format-string  [arguments...]
```
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```
* %s %c %d %f都是格式替代符
* %-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
* %-4.2f 指格式化为小数，其中.2指保留2位小数。

printf的转义序列

序列 | 说明
---|---
\a | 警告字符，通常为ASCII的BEL字符
\b | 后退
\c | 抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略
\f | 换页（formfeed）
\n | 换行
\r | 回车（Carriage return）
\t | 水平制表符
\v | 垂直制表符
\\ | 一个字面上的反斜杠字符
\ddd | 表示1到3位数八进制值的字符。仅在格式字符串中有效
\0ddd | 表示1到3位的八进制值字符

> %d %s %c %f 格式替代符详解:

* d: Decimal 十进制整数 -- 对应位置参数必须是十进制整数，否则报错！
* s: String 字符串 -- 对应位置参数必须是字符串或者字符型，否则报错！
* c: Char 字符 -- 对应位置参数必须是字符串或者字符型，否则报错！
* f: Float 浮点 -- 对应位置参数必须是数字型，否则报错！
* 如：其中最后一个参数是 "def"，%c 自动截取字符串的第一个字符作为结果输出。

# 8. test命令
# 9. 流程控制
> sh的流程控制不可为空
## 9.1. if 语句语法格式：
```
if condition
then
    command1 
    command2
    ...
    commandN 
fi

写成一行
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi


if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

## 9.2. for
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

for var in item1 item2 ... itemN; do command1; command2… done;
```

## 9.3. while
```
while condition
do
    command
done


#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```
> 使用中使用了 Bash let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量

```
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

## 9.4. 无限循环
```
while :
do
    command
done

while true
do
    command
done

for (( ; ; ))
```

## 9.5. until 循环
```
until condition
do
    command
done
```

## 9.6. case
```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

## 9.7. 跳出循环
* break
* continue
* 用两个分号表示break

# 10. 函数
## 10.1. 定义函数
```
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```
```
funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```
> 函数返回值在调用该函数后通过 $? 来获得。
> 所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。

## 10.2. 函数参数
> 通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数.
```
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```
> $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。
> 还有几个特殊字符用来处理参数：

参数处理 | 说明
-----|---
$# | 传递到脚本的参数个数
$* | 以一个单字符串显示所有向脚本传递的参数
$$ | 脚本运行的当前进程ID号
$! | 后台运行的最后一个进程的ID号
$@ | 与$*相同，但是使用时加引号，并在引号中返回每个参数。
$- | 显示Shell使用的当前选项，与set命令功能相同。
$? | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

# 11. 输入输出重定向
## 11.1. 重定向命令列表如下：

命令 | 说明
---|---
command > file | 将输出重定向到 file。
command < file | 将输入重定向到 file。
command >> file | 将输出以追加的方式重定向到 file。
n > file | 将文件描述符为 n 的文件重定向到 file。
n >> file | 将文件描述符为 n 的文件以追加的方式重定向到 file。
n >& m | 将输出文件 m 和 n 合并。
n <& m | 将输入文件 m 和 n 合并。
<< tag | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。

## 11.2. 重定向深入讲解
> 一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：
* 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
* 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
* 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。


* 如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：
```
 $ command > file 2>&1

或者

$ command >> file 2>&1
```

* Here Document
```
command << delimiter
    document
delimiter
```
> 它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。
> 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
> 开始的delimiter前后的空格会被忽略掉。
```
$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
$
```
* /dev/null 文件
> 如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：
```
$ command > /dev/null
```
> /dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。
> 如果希望屏蔽 stdout 和 stderr，可以这样写：

```
$ command > /dev/null 2>&1
```

> 这里的&没有固定的意思
> 放在>后面的&，表示重定向的目标不是一个文件，而是一个文件描述符，内置的文件描述符如下
* 1 => stdout
* 2 => stderr
* 0 => stdin
> 换言之 2>1 代表将stderr重定向到当前路径下文件名为1的regular file中，而2>&1代表将stderr重定向到文件描述符为1的文件(即/dev/stdout)中，这个文件就是stdout在file system中的映射
> 而&>file是一种特殊的用法，也可以写成>&file，二者的意思完全相同，都等价于 `>file 2>&1`
> 此处&>或者>&视作整体，分开没有单独的含义

# 12. 文件包含
```
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```