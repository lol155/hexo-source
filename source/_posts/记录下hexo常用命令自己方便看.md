---
title: 记录下hexo常用命令自己方便看
toc: true
date: 2017-09-03 22:02:00
categories: 工具
scaffolds:
tags: hexo
---

```
    Hexo常用命令：

        hexo new "postName"       #新建文章
        hexo new page "pageName"  #新建页面
        hexo generate             #生成静态页面至public目录
        hexo server               #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
        hexo deploy               #将.deploy目录部署到GitHub
```
<!-- more -->
简写
    
```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

复合命令

```
    hexo deploy -g
    hexo server -g
```    
有时候生成的网页出错了，而生成的rss其实没有清除，那么用下面的命令，在重新生成吧
    
    hexo clean