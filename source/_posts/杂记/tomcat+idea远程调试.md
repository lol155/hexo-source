---
title: tomcat+idea远程调试
categories: 学习
tags:
  - idea
toc: true
date: 2018-01-24 15:03:02
scaffolds:
---
# 测试的web项目
```java
package com.signalfire.servlet;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
/**
 * Created by jk on 2017/12/12.
 */
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("111111111111111111");
        resp.getWriter().write("hello baby");
        resp.getWriter().flush();
    }
}
```

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>my</servlet-name>
    <servlet-class>com.signalfire.servlet.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>my</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>
</web-app>
```
# tomcat
- 打war包部署到tomcat中
- 使用默认配置启动tomcat调试
```
[root@mytest bin]# ./catalina.sh jpda start
```
默认情况下，远程调试的默认端口为8000，可以通过JPDA_ADDRESS进行配置，指定自定义的端口，另外，还有两个可以配置的参数
* JPDA_TRANSPORT：即调试器和虚拟机之间数据的传输方式，默认值是dt_socket
* JPDA_SUSPEND：即JVM启动后是否立即挂起，默认是n
可以在catalina.sh中进行配置：
```bash
JPDA_TRANSPORT=dt_socket  
JPDA_ADDRESS=5005  
JPAD_SUSPEND=n  
```

# idea配置远程调试

## 配置  
![tomcat+idea远程调试-20171213104020](http://ovasdkxqr.bkt.clouddn.com/image/work/tomcat+idea远程调试-20171213104020.png)  
![tomcat+idea远程调试-2017121310403](http://ovasdkxqr.bkt.clouddn.com/image/work/tomcat+idea远程调试-2017121310403.png)  
## debug启动  
![tomcat+idea远程调试-20171213104146](http://ovasdkxqr.bkt.clouddn.com/image/work/tomcat+idea远程调试-20171213104146.png)

## 结果
![tomcat+idea远程调试-20171213104437](http://ovasdkxqr.bkt.clouddn.com/image/work/tomcat+idea远程调试-20171213104437.png)

![tomcat+idea远程调试-20171213104256](http://ovasdkxqr.bkt.clouddn.com/image/work/tomcat+idea远程调试-20171213104256.png)

# 调试java程序
在远程服务器上java启动参赛要加上调试的参数：  
"-Xdebug -Xrunjdwp:transport=dt_socket,address=2345,server=y,suspend=n"