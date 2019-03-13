---
title: log4j打印sql语句和参数
categories: 杂记
tags:
  - 日志
  - log4j
toc: true
date: 2019-03-13 12:10:00
scaffolds:
---
在配置文件中加入
```
log4j.logger.org.springframework.jdbc=debug
log4j.logger.org.springframework.jdbc.core.StatementCreatorUtils=TRACE
```