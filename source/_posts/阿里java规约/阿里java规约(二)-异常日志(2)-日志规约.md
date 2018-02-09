---
title: 阿里java规约(二)-异常日志(2)-日志规约.md
categories: 阿里java规约
tags:
  - 阿里java规约
toc: true
date: 2018-02-09 16:29:33
scaffolds:
---
# 日志规约(8)
## 强制(6)
1. 【强制】应用中`不可直接使用`日志系统（ Log4j、 Logback） 中的 API，而应依赖`使用`日志框架`SLF4J` 中的 API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。
    ```java
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    private static final Logger logger = LoggerFactory.getLogger(Abc.class);
    ```
2. 【强制】日志文件推荐`至少`保存 `15` 天，因为有些异常具备以“周”为频次发生的特点。
3. 【强制】应用中的扩展日志（ 如打点、临时监控、访问日志等） 命名方式：
    * appName_logType_logName.log。
    * logType:日志类型，推荐分类有 stats/monitor/visit 等；
    * logName:日志描述。这种命名的好处：通过文件名就可知道日志文件属于什么应用，什么类型，什么目的，也有利于归类查找。
    * 正例： mppserver 应用中单独监控时区转换异常，如：mppserver_monitor_timeZoneConvert.log
    * 说明： 推荐对日志进行分类， 如将错误日志和业务日志分开存放，便于开发人员查看，也便于通过日志对系统进行及时监控。
4. 【强制】对 trace/debug/info 级别的日志输出，必须使用条件输出形式或者使用占位符的方式。
    * 说明： logger.debug("Processing trade with id: " + id + " and symbol: " + symbol);
    * 如果日志级别是 warn，上述日志不会打印，但是会执行字符串拼接操作，如果 symbol 是对象，会执行 toString()方法，浪费了系统资源，执行了上述操作，最终日志却没有打印。
    * 正例： （ 条件）
        ```java
        if (logger.isDebugEnabled()) {
            logger.debug("Processing trade with id: " + id + " and symbol: " + symbol);
        }
        ```
    * 正例： （ 占位符）
        ```java
        logger.debug("Processing trade with id: {} and symbol : {} ", id, symbol);
        ```
5. 【强制】避免重复打印日志，浪费磁盘空间，务必在 log4j.xml 中设置 additivity=false。
    * 正例： <logger name="com.taobao.dubbo.config" additivity="false">
6. 【强制】异常信息应该包括两类信息：`案发现场信息`和`异常堆栈信息`。如果不处理，那么通过关键字 throws 往上抛出。
    * 正例： logger.error(各类参数或者对象 toString + "_" + e.getMessage(), e);

## 推荐(2)
7. 【推荐】谨慎地记录日志。生产环境禁止输出 debug 日志； 有选择地输出 info 日志； 如果使用 warn 来记录刚上线时的业务行为信息，一定要注意日志输出量的问题，避免把服务器磁盘撑爆，并记得及时删除这些观察日志。
    * 说明： 大量地输出无效日志，不利于系统性能提升，也不利于快速定位错误点。 记录日志时请思考：这些日志真的有人看吗？看到这条日志你能做什么？能不能给问题排查带来好处？
8. 【推荐】可以使用 `warn` 日志级别来记录`用户输入参数错误`的情况，避免用户投诉时，无所适从。如非必要，请不要在此场景打出 error 级别，避免频繁报警。
    * 说明： 注意日志输出的级别， error 级别只记录系统逻辑出错、异常或者重要的错误信息。