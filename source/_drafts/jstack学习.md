---
title: jstack学习
categories: jdk工具
tags:
  - jdk-tool
---
# 1. jstack学习
https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jstack.html#BABHGEEI
## 1.1. 命令
- jstack [ options ] pid
- jstack [ options ] executable core
- jstack [ options ] [ server-id@ ] remote-hostname-or-IP

- options
- pid
- executable
- core
- remote-hostname-or-IP
- server-id

## 1.2. options
- -F
Force a stack dump when jstack [-l] pid does not respond.

- -l
Long listing. Prints additional information about locks such as a list of owned java.util.concurrent ownable synchronizers. See the AbstractOwnableSynchronizer class description at http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/AbstractOwnableSynchronizer.html

- -m
Prints a mixed mode stack trace that has both Java and native C/C++ frames.

- -h
Prints a help message.

- -help
Prints a help message.

# jps
jps [ options ] [ hostid ]

- -q
Suppresses the output of the class name, JAR file name, and arguments passed to the main method, producing only a list of local JVM identifiers.

- -m
Displays the arguments passed to the main method. The output may be null for embedded JVMs.

- -l
Displays the full package name for the application's main class or the full path name to the application's JAR file.

- -v
Displays the arguments passed to the JVM.

- -V
Suppresses the output of the class name, JAR file name, and arguments passed to the main method, producing only a list of local JVM identifiers.

- -Joption
Passes option to the JVM, where option is one of the options described on the reference page for the Java application launcher. For example, -J-Xms48m sets the startup memory to 48 MB. See java(1).