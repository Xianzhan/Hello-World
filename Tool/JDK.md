<!-- TOC -->

- [构建程序](#构建程序)
    - [appletviewer](#appletviewer)
    - [extcheck](#extcheck)
    - [jar](#jar)
    - [java](#java)
    - [javac](#javac)
    - [javadoc](#javadoc)
    - [javah](#javah)
    - [javap](#javap)
    - [jdb](#jdb)
    - [jdeps](#jdeps)
- [监控 Java 应用](#监控-java-应用)
    - [jconsole](#jconsole)
    - [jvisualvm](#jvisualvm)
- [监控虚拟机](#监控虚拟机)
    - [jps](#jps)
    - [jstat](#jstat)
    - [jstatd](#jstatd)
    - [jmc](#jmc)
- [故障排除](#故障排除)
    - [jcmd](#jcmd)
    - [jinfo](#jinfo)
    - [jhat](#jhat)
    - [jmap](#jmap)
    - [jsadebugd](#jsadebugd)
    - [jstack](#jstack)
- [部署应用和 Applets](#部署应用和-applets)
    - [pack200](#pack200)
    - [unpack200](#unpack200)
    - [javapackager](#javapackager)
    - [~~javafxpackager~~](#javafxpackager)
- [安全](#安全)
    - [keytool](#keytool)
    - [jarsigner](#jarsigner)
    - [policytool](#policytool)
    - [kinit](#kinit)
    - [klist](#klist)
    - [ktab](#ktab)
- [国际化](#国际化)
    - [native2ascii](#native2ascii)
- [远程方法调用](#远程方法调用)
    - [rmic](#rmic)
    - [rmiregistry](#rmiregistry)
    - [rmid](#rmid)
    - [serialver](#serialver)
- [Java IDL and RMI-IIOP](#java-idl-and-rmi-iiop)
    - [tnameserv](#tnameserv)
    - [idlj](#idlj)
    - [orbd](#orbd)
    - [servertool](#servertool)
- [Java Web Start](#java-web-start)
    - [javaws](#javaws)
- [网络服务](#网络服务)
    - [schemagen](#schemagen)
    - [wsgen](#wsgen)
    - [wsimport](#wsimport)
    - [xjc](#xjc)
- [脚本](#脚本)
    - [jrunscript](#jrunscript)
    - [jjs](#jjs)

<!-- /TOC -->

# 构建程序

## appletviewer

> 在Web浏览器之外运行 applet。

## extcheck

> 检测目标 Java Archive（JAR）文件与当前安装的扩展 JAR 文件之间的版本冲突。

## jar

> 处理 Java Archive（JAR）文件。

## java

> 启动 Java 应用程序。

## javac

> 读取 Java 类和接口定义，并将它们编译为字节码和类文件。

## javadoc

> 从 Java 源文件生成 API 文档的 HTML 页面。

## javah

> 从 Java 类生成 C 头文件和源文件。

## javap

> 反汇编一个或多个类文件。

## jdb

> 查找并修复 Java 平台程序中的错误。

## jdeps

> Java 类依赖性分析器。

# 监控 Java 应用

## jconsole

> 启动一个图形控制台，可以监视和管理 Java 应用程序。

## jvisualvm

> 对 Java 应用程序进行可视化监视，故障排除和配置文件。

# 监控虚拟机

## jps

> 列出目标系统上的已检测 Java 虚拟机（JVM）。此命令是实验性的，不受支持。

## jstat

> 监视 Java 虚拟机（JVM）统计信息。此命令是实验性的，不受支持。

## jstatd

> 监视 Java 虚拟机（JVM）并使远程监视工具能够连接到 JVM。此命令是实验性的，不受支持。

## jmc

> Java Mission Control 是一个分析，监视和诊断工具套件。

# 故障排除

## jcmd

> 将诊断命令请求发送到正在运行的 Java 虚拟机（JVM）。

## jinfo

> 生成配置信息。此命令是实验性的，不受支持。

## jhat

> 分析 Java 堆。此命令是实验性的，不受支持。

## jmap

> 打印进程，核心文件或远程调试服务器的共享对象内存映射或堆内存详细信息。此命令是实验性的，不受支持。

## jsadebugd

> 附加到 Java 进程或核心文件，并充当调试服务器。此命令是实验性的，不受支持。

## jstack

> 打印 Java 进程，核心文件或远程调试服务器的 Java 线程堆栈跟踪。此命令是实验性的，不受支持。

# 部署应用和 Applets

## pack200

> 将 JAR 文件打包到压缩的 pack200 文件中以进行 Web 部署。

## unpack200

> 将 `pack200` 生成的打包文件转换为 JAR 文件以进行 Web 部署。

## javapackager

> 执行与打包和签名 Java 和 JavaFX 应用程序相关的任务。

## ~~javafxpackager~~

> 注意：此工具已重命名为 `javapackager`。可能会在将来的发行版中删除 javafxpackager.exe 文件。请更新您的脚本以使用 `javapackager`。

> 执行与打包和签名 Java 和 JavaFX 应用程序相关的任务。

# 安全

## keytool

> 管理加密密钥，X.509 证书链和可信证书的密钥库（数据库）。

## jarsigner

> 签署并验证 Java Archive（JAR）文件。

## policytool

> 通过实用程序 GUI 基于用户输入读取和写入纯文本策略文件。

## kinit

> 获取并缓存 Kerberos 票据授予票据。这个工具在功能上与其他 Kerberos 实现(如 SEAM 和 MIT 参考实现)中常见的 `kinit` 工具类似。在运行 `kinit` 之前，用户必须在密钥分发中心(KDC)注册为主体。

## klist

> 使您可以查看本地凭据缓存和密钥表中的条目。

## ktab

> 允许用户管理存储在本地密钥表中的主体名称和服务密钥。keytab 中列出的主体和密钥对允许在主机上运行的服务对密钥分发中心(KDC)进行身份验证。在配置使用 Kerberos 的服务器之前，必须在运行服务器的主机上设置一个 keytab。注意，使用 `ktab` 工具对 keytab 进行的任何更新都不会影响 Kerberos 数据库。如果更改 keytab 中的键，还必须对 Kerberos 数据库进行相应的更改。

# 国际化

## native2ascii

> 通过将具有任何支持的字符编码的字符的文件转换为具有 ASCII 或 Unicode 转义的文件来创建可本地化的应用程序，反之亦然。

```bash
native2ascii -encoding UTF-8 i18n_zh_CN.properties i18n_ascii.properties
```

# 远程方法调用

## rmic

> 为使用 Java 远程方法协议（JRMP）或 Internet Inter-Orb 协议（IIOP）的远程对象生成存根，框架和绑定类。还生成对象管理组（OMG）接口定义语言（IDL）

## rmiregistry

> 在当前主机上的指定端口上启动远程对象注册表。

## rmid

> 启动激活系统守护程序，该守护程序允许在 Java 虚拟机（JVM）中注册和激活对象。

## serialver

> 返回指定类的串行版本 UID。

# Java IDL and RMI-IIOP

## tnameserv

> 接口定义语言（IDL）。

## idlj

> 为指定的接口定义语言（IDL）文件生成 Java 绑定。

## orbd

> 使客户端能够在 CORBA 环境中的服务器上查找和调用持久对象。

## servertool

> 为开发人员提供易于使用的界面，以注册，取消注册，启动和关闭持久性服务器。

# Java Web Start

## javaws

> 启动 Java Web Start。

# 网络服务

## schemagen

> 为 Java 类中引用的每个名称空间生成模式。

## wsgen

> 读取 Web 服务端点实现（SEI）类，并为 Web 服务部署和调用生成所有必需的工件。

## wsimport

> 生成可以打包在 Web 应用程序归档（WAR）文件中的 JAX-WS 可移植工件，并提供 Ant 任务。

## xjc

> 将 XML 模式文件编译为完全注释的 Java 类。

# 脚本

## jrunscript

> 运行支持交互式和批处理模式的命令行脚本 shell。此命令是实验性的，不受支持。

## jjs

> 调用 Nashorn 引擎。