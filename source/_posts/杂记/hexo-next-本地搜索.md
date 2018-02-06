---
title: hexo-next 本地搜索
categories: 学习
tags:
  - hexo
toc: true
date: 2017-11-14 23:46:14
scaffolds:
---
之前用的algolia用不了了，应该是收费。查找了帖子做了本地搜索，感觉也很不错，主要是不用担心不好使了。
# 1. local search
## 1.1. 安装hexo-generator-searchdb
在站点根目录通过gitbash安装
```
npm install hexo-generator-searchdb --save
```
<!--more-->
## 1.2. 添加search字段
在站点下_config.yml中添加search字段
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
## 1.3. 在themes\next\_config.yml主题配置中找到
```
local_search:
  enable: true
```
将enable的值改成true

# 2. 参考链接
[Hexo的Next主题配置](https://www.cnblogs.com/syd192/p/6074323.html)