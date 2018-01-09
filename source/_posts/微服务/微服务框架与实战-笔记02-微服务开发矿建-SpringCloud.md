---
title: 微服务框架与实战-笔记02-微服务开发矿建-SpringCloud
categories: 学习
tags:
  - 大数据
toc: true
date: 2018-01-09 23:12:17
scaffolds:
---

# Spring Cloud 简介
- 在SpringBoot基础上构建的，用于快速构建分布式系统的通用模式工具集。
- 程序适合在Docker或者PaaS上部署。 所以又叫`云原生应用`（Cloud Native Application）。
- `云原生`（CloudNative）可简单理解为面向云环境的软件架构。
# Spring Cloud 特点
- 约定优于配置
- 适用于各种环境
- 隐藏了组件的复杂性，并提供声明式、无xml的配置方式
- 开箱即用、快速启动
- 轻量级组件
- 组件丰富，功能齐全。如 配置管理，服务发现，断路器，微服务网关等
- 选型中立、丰富。例如：支持使用Eureka、Zookeeper、Consul实现服务发现。
- 灵活。组成部分解耦，可按需挑选技术选型。
# 版本
Spring项目一般以下面这种方式命名，但`SpringCloud`并`没有`使用这种方式。可以略过。
主版本号.次版本号.增量版本号.里程碑版本号  
例如：4.3.5.RELEASE
- 主版本号：项目重大重构
- 次版本号：新特性的添加和变化
- 增量版本号：一般表示BUG修复
- 里程碑版本号：某个版本号的里程碑
## 版本简介
![20181923037](http://ovasdkxqr.bkt.clouddn.com/image/blog/20181923037.png)
命名方式：英文单词SRX（x为数字）
英文单词：release train 
SR：service release bug修复

Dalston SR5 表示Dalston 第5次bug修复版本

https://github.com/spring-cloud/spring-cloud-release/releases 版本发布

## 子项目一览

Component | Camden.SR7 | Dalston.SR4 | Edgware.RELEASE | Finchley.M4 | Finchley.BUILD-SNAPSHOT
----------|------------|-------------|-----------------|-------------|------------------------
spring-cloud-aws | 1.1.4.RELEASE | 1.2.1.RELEASE | 1.2.2.RELEASE | 2.0.0.M2 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-bus | 1.2.2.RELEASE | 1.3.1.RELEASE | 1.3.2.RELEASE | 2.0.0.M3 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-cli | 1.2.4.RELEASE | 1.3.4.RELEASE | 1.4.0.RELEASE | 2.0.0.M1 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-commons | 1.1.9.RELEASE | 1.2.4.RELEASE | 1.3.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-contract | 1.0.5.RELEASE | 1.1.4.RELEASE | 1.2.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-config | 1.2.3.RELEASE | 1.3.3.RELEASE | 1.4.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-netflix | 1.2.7.RELEASE | 1.3.5.RELEASE | 1.4.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-security | 1.1.4.RELEASE | 1.2.1.RELEASE | 1.2.1.RELEASE | 2.0.0.M1 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-cloudfoundry | 1.0.1.RELEASE | 1.1.0.RELEASE | 1.1.0.RELEASE | 2.0.0.M1 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-consul | 1.1.4.RELEASE | 1.2.1.RELEASE | 1.3.0.RELEASE | 2.0.0.M3 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-sleuth | 1.1.3.RELEASE | 1.2.5.RELEASE | 1.3.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-stream | Brooklyn.SR3 | Chelsea.SR2 | Ditmars.RELEASE | Elmhurst.M3 | Elmhurst.BUILD-SNAPSHOT
spring-cloud-zookeeper | 1.0.4.RELEASE | 1.1.2.RELEASE | 1.2.0.RELEASE | 2.0.0.M3 | 2.0.0.BUILD-SNAPSHOT
spring-boot | 1.4.5.RELEASE | 1.5.4.RELEASE | 1.5.8.RELEASE | 2.0.0.M6 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-task | 1.0.3.RELEASE | 1.1.2.RELEASE | 1.2.2.RELEASE | 2.0.0.M2 | 2.0.0.RELEASE
spring-cloud-vault |   | 1.0.2.RELEASE | 1.1.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT
spring-cloud-gateway |   |   | 1.0.0.RELEASE | 2.0.0.M4 | 2.0.0.BUILD-SNAPSHOT


## spring cloud / spring boot 版本兼容性

- Finchley使用Spring Boot 2.0.x构建和运行，并且不希望与Spring Boot 1.5.x一起使用。
- Dalston和Edgware发行版建立在Spring Boot 1.5.x之上，并且不希望与Spring Boot 2.0.x一起使用。
- Camden发行版基于Spring Boot 1.4.x，但也使用1.5.x进行测试。

http://projects.spring.io/spring-cloud/ 更具体的额可以看这里