<!-- TOC -->

- [构建程序](#构建程序)
    - [~~appletviewer~~](#appletviewer)
    - [~~extcheck~~](#extcheck)
    - [jar](#jar)
    - [java](#java)
    - [javac](#javac)
    - [javadoc](#javadoc)
    - [~~javah~~](#javah)
    - [javap](#javap)
    - [jdeprscan](#jdeprscan)
    - [jdeps](#jdeps)
    - [jlink](#jlink)
    - [jmod](#jmod)
- [监控 Java 应用](#监控-java-应用)
    - [jconsole](#jconsole)
    - [~~jvisualvm~~](#jvisualvm)
- [监控虚拟机](#监控虚拟机)
    - [jps](#jps)
    - [jstat](#jstat)
    - [jstatd](#jstatd)
    - [~~jmc~~](#jmc)
- [故障排除](#故障排除)
    - [jcmd](#jcmd)
    - [jdb](#jdb)
    - [jhsdb](#jhsdb)
    - [jinfo](#jinfo)
    - [~~jhat~~](#jhat)
    - [jmap](#jmap)
    - [~~jsadebugd~~](#jsadebugd)
    - [jstack](#jstack)
- [Java 辅助功能](#java-辅助功能)
    - [jaccessinspector](#jaccessinspector)
    - [jaccesswalker](#jaccesswalker)
- [Language Shell](#language-shell)
    - [jshell](#jshell)
- [脚本](#脚本)
    - [jjs](#jjs)
    - [jrunscript](#jrunscript)
- [安全](#安全)
    - [keytool](#keytool)
    - [jarsigner](#jarsigner)
    - [~~policytool~~](#policytool)
    - [kinit](#kinit)
    - [klist](#klist)
    - [ktab](#ktab)
- [远程方法调用](#远程方法调用)
    - [rmic](#rmic)
    - [rmiregistry](#rmiregistry)
    - [rmid](#rmid)
    - [serialver](#serialver)
- [~~部署应用和 Applets~~](#部署应用和-applets)
    - [~~pack200~~](#pack200)
    - [~~unpack200~~](#unpack200)
    - [~~javapackager~~](#javapackager)
    - [~~javafxpackager~~](#javafxpackager)
- [~~国际化~~](#国际化)
    - [~~native2ascii~~](#native2ascii)
- [~~Java IDL and RMI-IIOP~~](#java-idl-and-rmi-iiop)
    - [~~tnameserv~~](#tnameserv)
    - [~~idlj~~](#idlj)
    - [~~orbd~~](#orbd)
    - [~~servertool~~](#servertool)
- [~~Java Web Start~~](#java-web-start)
    - [~~javaws~~](#javaws)
- [~~网络服务~~](#网络服务)
    - [~~schemagen~~](#schemagen)
    - [~~wsgen~~](#wsgen)
    - [~~wsimport~~](#wsimport)
    - [~~xjc~~](#xjc)

<!-- /TOC -->

# 构建程序

## ~~appletviewer~~

> @delete 11<br>

> 在Web浏览器之外运行 applet。

## ~~extcheck~~

> @delete 9<br>

> 检测目标 Java Archive（JAR）文件与当前安装的扩展 JAR 文件之间的版本冲突。

## jar

> 处理 Java Archive（JAR）文件。

## java

> 启动 Java 应用程序。

## javac

> 读取 Java 类和接口定义，并将它们编译为字节码和类文件。

## javadoc

> 从 Java 源文件生成 API 文档的 HTML 页面。

## ~~javah~~

> @delete 11<br>
> @see [javac](#javac)

> 从 Java 类生成 C 头文件和源文件。

## javap

> 反汇编一个或多个类文件。

## jdeprscan

> @since 9

> 您可以使用 `jdeprscan` 工具作为静态分析工具来扫描 jar 文件（或其他类文件聚合）以使用已弃用的 API 元素。

## jdeps

> Java 类依赖性分析器。

## jlink

> @since 10<br>

> 您可以使用 `jlink` 工具将一组模块及其依赖项组合和优化到自定义运行时映像中。

## jmod

> @since 9<br>

> 您可以使用 `jmod` 工具创建 JMOD 文件并列出现有 JMOD 文件的内容。

# 监控 Java 应用

## jconsole

> 启动一个图形控制台，可以监视和管理 Java 应用程序。

## ~~jvisualvm~~

> @delete 9

> 对 Java 应用程序进行可视化监视，故障排除和配置文件。

# 监控虚拟机

## jps

> 列出目标系统上的已检测 Java 虚拟机（JVM）。此命令是实验性的，不受支持。

## jstat

> 监视 Java 虚拟机（JVM）统计信息。此命令是实验性的，不受支持。

## jstatd

> 监视 Java 虚拟机（JVM）并使远程监视工具能够连接到 JVM。此命令是实验性的，不受支持。

## ~~jmc~~

> @delete 11<br>

> Java Mission Control 是一个分析，监视和诊断工具套件。

# 故障排除

## jcmd

> 将诊断命令请求发送到正在运行的 Java 虚拟机（JVM）。

## jdb

> 查找并修复 Java 平台程序中的错误。

## jhsdb

> @since 9<br>

> 您可以使用 `jhsdb` 工具附加到 Java 进程或启动事后调试程序，以便从崩溃的 Java 虚拟机（JVM）中分析核心转储的内容。

## jinfo

> 生成配置信息。此命令是实验性的，不受支持。

## ~~jhat~~

> @delete 9<br>

> 分析 Java 堆。此命令是实验性的，不受支持。

## jmap

> 打印进程，核心文件或远程调试服务器的共享对象内存映射或堆内存详细信息。此命令是实验性的，不受支持。

## ~~jsadebugd~~

> @delete 9<br>

> 附加到 Java 进程或核心文件，并充当调试服务器。此命令是实验性的，不受支持。

## jstack

> 打印 Java 进程，核心文件或远程调试服务器的 Java 线程堆栈跟踪。此命令是实验性的，不受支持。

# Java 辅助功能

## jaccessinspector 

> @since 9<br>

> 您可以使用 Java Accessibility Utilities API 的 `jaccessinspector` 辅助功能评估工具来检查有关 Java 虚拟机中对象的可访问信息。

## jaccesswalker

> @since 9<br>

> 您可以使用 `jaccesswalker` 浏览特定 Java 虚拟机中的组件树，并在树视图中显示层次结构。

# Language Shell

## jshell

> @since 9<br>

> 您可以使用jshell工具在read-eval-print loop（REPL）中以交互方式评估 Java 编程语言的声明，语句和表达式。

# 脚本

## jjs

> 调用 Nashorn 引擎。

## jrunscript

> 运行支持交互式和批处理模式的命令行脚本 shell。此命令是实验性的，不受支持。

# 安全

## keytool

> 管理加密密钥，X.509 证书链和可信证书的密钥库（数据库）。

## jarsigner

> 签署并验证 Java Archive（JAR）文件。

## ~~policytool~~

> @delete 10<br>

> 通过实用程序 GUI 基于用户输入读取和写入纯文本策略文件。

## kinit

> 获取并缓存 Kerberos 票据授予票据。这个工具在功能上与其他 Kerberos 实现(如 SEAM 和 MIT 参考实现)中常见的 `kinit` 工具类似。在运行 `kinit` 之前，用户必须在密钥分发中心(KDC)注册为主体。

## klist

> 使您可以查看本地凭据缓存和密钥表中的条目。

## ktab

> 允许用户管理存储在本地密钥表中的主体名称和服务密钥。keytab 中列出的主体和密钥对允许在主机上运行的服务对密钥分发中心(KDC)进行身份验证。在配置使用 Kerberos 的服务器之前，必须在运行服务器的主机上设置一个 keytab。注意，使用 `ktab` 工具对 keytab 进行的任何更新都不会影响 Kerberos 数据库。如果更改 keytab 中的键，还必须对 Kerberos 数据库进行相应的更改。

# 远程方法调用

## rmic

> 为使用 Java 远程方法协议（JRMP）或 Internet Inter-Orb 协议（IIOP）的远程对象生成存根，框架和绑定类。还生成对象管理组（OMG）接口定义语言（IDL）

## rmiregistry

> 在当前主机上的指定端口上启动远程对象注册表。

## rmid

> 启动激活系统守护程序，该守护程序允许在 Java 虚拟机（JVM）中注册和激活对象。

## serialver

> 返回指定类的串行版本 UID。

# ~~部署应用和 Applets~~

## ~~pack200~~

> @deprecated 11<br>

> 将 JAR 文件打包到压缩的 pack200 文件中以进行 Web 部署。

## ~~unpack200~~

> @deprecated 11<br>

> 将 `pack200` 生成的打包文件转换为 JAR 文件以进行 Web 部署。

## ~~javapackager~~

> @delete 11<br>

> 执行与打包和签名 Java 和 JavaFX 应用程序相关的任务。

## ~~javafxpackager~~

> @deprecated 8<br>
> @delete 9<br>

> 注意：此工具已重命名为 `javapackager`。可能会在将来的发行版中删除 javafxpackager.exe 文件。请更新您的脚本以使用 `javapackager`。

> 执行与打包和签名 Java 和 JavaFX 应用程序相关的任务。

# ~~国际化~~

## ~~native2ascii~~

> @delete 9<br>

> 通过将具有任何支持的字符编码的字符的文件转换为具有 ASCII 或 Unicode 转义的文件来创建可本地化的应用程序，反之亦然。

```bash
native2ascii -encoding UTF-8 i18n_zh_CN.properties i18n_ascii.properties
```

# ~~Java IDL and RMI-IIOP~~

## ~~tnameserv~~

> @delete 11<br>

> 接口定义语言（IDL）。

## ~~idlj~~

> @delete 11<br>

> 为指定的接口定义语言（IDL）文件生成 Java 绑定。

## ~~orbd~~

> @delete 11<br>

> 使客户端能够在 CORBA 环境中的服务器上查找和调用持久对象。

## ~~servertool~~

> @delete 11<br>

> 为开发人员提供易于使用的界面，以注册，取消注册，启动和关闭持久性服务器。

# ~~Java Web Start~~

## ~~javaws~~

> @delete 11<br>

> 启动 Java Web Start。

# ~~网络服务~~

## ~~schemagen~~

> @delete 11<br>

> 为 Java 类中引用的每个名称空间生成模式。

## ~~wsgen~~

> @delete 11<br>

> 读取 Web 服务端点实现（SEI）类，并为 Web 服务部署和调用生成所有必需的工件。

## ~~wsimport~~

> @delete 11<br>

> 生成可以打包在 Web 应用程序归档（WAR）文件中的 JAX-WS 可移植工件，并提供 Ant 任务。

## ~~xjc~~

> @delete 11<br>

> 将 XML 模式文件编译为完全注释的 Java 类。
