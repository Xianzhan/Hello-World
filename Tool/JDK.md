<!-- TOC -->

- [查阅](#查阅)
    - [javap](#javap)
    - [jps](#jps)
- [诊断](#诊断)
    - [jcmd](#jcmd)
    - [jhat](#jhat)
    - [jinfo](#jinfo)
    - [jmap](#jmap)
    - [jmc](#jmc)
    - [jstack](#jstack)
    - [jstat](#jstat)
    - [visualvm](#visualvm)

<!-- /TOC -->

# 查阅

## javap

查阅 Java 字节码

## jps

打印所有正在运行的 Java 进程。默认输出 Java 进程的 ID 以及主类名。

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

可以用来打印目标 Java 进程中各个线程的栈轨迹，以及这些线程所持有的锁。

## jstat

允许用户查看目标 Java 进程的类加载、即时编译以及垃圾回收相关的信息。它常用于检测垃圾回收问题以及内存泄露问题。

## visualvm

通过 JMX 接口连接 JVM 进程, 从而能够看到 JVM 上的线程、内存、类等信息. 可以安装各种插件. (通过 CATALINA_OPTS 开启 Tomcat jmx 接口)