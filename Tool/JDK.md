[诊断](#诊断)
 - [jcmd](#jcmd)
 - [jhat](#jhat)
 - [jinfo](#jinfo)
 - [jmap](#jmap)
 - [jmc](#jmc)
 - [jstack](#jstack)
 - [jstat](#jstat)
 - [visualvm](#visualvm)

# 诊断

## jcmd

Java 命令行(Java Command), 用于向正在运行的 JVM 发送诊断命令请求. 由于 jmap 官方标注的是 unsupported, jcmd 可以作为其替代工具

## jhat

Java 堆分析工具(Java Heap Analysis Tool), 用于分析 Java 堆内存中的对象信息

## jinfo

Java 配置信息工具(Java Configuration Information), 用于打印指定 Java 进程、核心文件或远程调试服务器的配置信息, 也可以动态修改 JVM 参数配置

## jmap

Java 内存映射工具(Java Memory Map), 主要用于打印指定 Java 进程、核心文件或远程调试服务器的共享对象内存映射或堆内存细节

## jmc

Java Mission Control, 是一款采样型的集诊断、分析和监控于一体的非常强大的工具. 由于收费的原因, 用的不多

## jstack

Java 堆栈跟踪工具, 主要用于打印指定 Java 进程、核心文件或远程调试服务器的 Java 线程的堆栈跟踪信息

## jstat

JVM 统计监测工具(Java Statistics Monitoring Tool), 主要用于监测并显示 JVM 的性能统计信息, 包括 GC 统计信息

## visualvm

通过 JMX 接口连接 JVM 进程, 从而能够看到 JVM 上的线程、内存、类等信息. 可以安装各种插件. (通过 CATALINA_OPTS 开启 Tomcat jmx 接口)