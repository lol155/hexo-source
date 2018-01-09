---
title: 使用多个git账户
categories: 学习
tags:
  - git
toc: true
date: 2018-01-07 17:36:01
scaffolds:
---

# 生成两个SSH key
为了举例方便，这里使用“one”和“two”两个账户。下同。
```
$ ssh-keygen -t rsa -C "one@gmail.com"

$ ssh-keygen -t rsa -C "two@gmail.com"
```
不要一路回车，分别在第一个对话的时候输入重命名（id_rsa_one和id_rsa_two），这样会生成两份，包含私钥和公钥的4个文件。
```
id_rsa_one
id_rsa_one.pub
id_rsa_two
id_rsa_two.pub
```
# 添加私钥

1. 打开ssh-agent

    - 如果你是github官方的bash：
    ```
    $ ssh-agent -s
    ```
    - 如果你是其它，比如msysgit：
    ```
    $ eval $(ssh-agent -s)
    ```

2. 添加私钥

    ```
    ssh-agent bash  //不执行这个会报没有权限
    $ ssh-add ~/.ssh/id_rsa_one
    $ ssh-add ~/.ssh/id_rsa_two
    ```
# 创建config文件
在.ssh下创建config

```
    # one(one@gmail.com) 
    Host one.github.com //自定义的映射
    HostName github.com //git仓库对应的地址
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_one //对应为文件
    User one   //对应的用户名
    # two(two@ gmail.com)
    Host two.github.com
    HostName github.com   
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_two
    User two
```
# 部署SSH key
在github上添加对应的ssh key
# clone项目方法
> $ git clone git@`one.github.com`: `one的用户名`/learngit.git  注意是自定义的域名 和用户名（git的用户名）  
> $ git clone git@`two.github.com`: `two的用户名`/learngit.git 

# 其他（与上面没有关联）
##  git的三种环境变量
1. 系统变量。
    - 存放在git的安装目录下：%Git%\etc\gitconfig。
    - 若使用 git config 时用 --system 选项，读写的就是这个文件：
    - $ git config --system core.symlinks
    - 系统变量对所有用户都适用。
2. 用户变量。
    - 存放在用户目录下。例如windows xp存放在：C:\Documents and Settings\$USER\.gitconfig。
    - 若使用 git config 时用 --global 选项，读写的就是这个文件：
    - $ git config --global user.name
    - 用户变量只适用于该用户
3. 本地项目变量
    - 当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）。
    - 若使用git config 时用 --local 选项，读写的就是这个文件：
    - $ git config --local remote.origin.url
    - 本地变量只对当前项目有效。

## 查找顺序
本地 》 用户 》系统
## 其他config命令
```
$ git config --list 查看所有环境变量
$ git config --system --list 查看系统环境变量
$ git config --global --list 查看用户环境变量
$ git config --local --list 查看本地环境变量
$ git config --[system/global/local] [varname] [yourname] 编辑环境变量
```