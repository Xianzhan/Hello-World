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

**结构**

```bash
javac [option] [sourcefiles] [@files]
# 参数可按任意次序排列
# option 命令行选项
# sourcefiles 一个或多个要编译的源文件（例如 Hello.java）
# @files 一个或多个对源文件进行列表的文件
```

**说明**

有两种方法可以将源代码文件名传递给 `javac`:

1. 如果源文件数量少，在命令行上列出文件名即可。
2. 如果源文件数量多，则将源文件列在一个文件中，名称间用空格或者回车进行分割。然后再 `javac` 命令行中使用该列表文件名，文件名前冠以 `@` 字符。

源代码文件名必须含有 `.java` 后缀，类文件名称必须含有 `.class` 后缀，源文件和类文件都必须有识别该类的根名。

*例如*：

名为 `Hello` 的类将写在名为 `Hello.java` 的源文件中，并编译为字节码文件 `Hello.class`。<br>
内部类定义产生附加的类文件。这些类文件的名称将内部类和外部类的名称结合在一起，如：`Hello$InnerClass.class`。

应当将源文件安排在反映其包树结构的目录树中。

*例如*：

如果将所有源文件放在 `/workspace` 目录中，那么 `xianzhan.Hello` 的代码应该在 `/workspace/xianzhan/Hello.java` 中。<br>
default 情况下，编译器将每个类文件与其源文件放在同一目中。可用 `-d` 选项指定其它目标目录。

**查找类型**

当编译源文件时，编译器常常需要它还没有识别出的类型的有关信息。对于源文件中使用、扩展或实现的每个类或接口，编译器都需要其类型信息。这包括在源文件中没有明确提及、但通过继承提供信息的类和接口。

例如，当扩展 `java.applet.Applet` 时还要用到 Applet 的祖先类：

```java
java.awt.Panel
java.awt.Container
java.awt.Component
java.awt.Object
```

当编译器需要类型信息时，它将查找定义类型的源文件或类文件。编译器先在自举类及扩展类中查找，然后在用户类路径中查找。用户类路径通过两种途径来定义：通过设置 **CLASSPATH** 环境变量或使用 `-classpath` 命令行选项。（有关详细资料，请参阅设置类路径）。如果使用 `-sourcepath` 选项，则编译器在 **sourcepath** 指定的路径中查找源文件；否则，编译器将在用户类路径中查找类文件和源文件。可用`-bootclasspath` 和 `-extdirs` 选项来指定不同的自举类或扩展类；参阅下面的联编选项。

成功的类型搜索可能生成类文件、源文件或两者兼有。以下是 `javac` 对各种情形所进行的处理：

- 搜索结果只生成类文件而没有源文件： `javac` 使用类文件。
- 搜索结果只生成源文件而没有类文件： `javac` 编译源文件并使用由此生成的类文件。
- 搜索结果既生成源文件又生成类文件： 确定类文件是否过时。若类文件已过时，则 `javac` 重新编译源文件并使用更新后的类文件。否则， `javac` 直接使用类文件。

缺省情况下，只要类文件比源文件旧， `javac` 就认为它已过时。（`-Xdepend` 选项指定相对来说较慢但却比较可靠的过程。）

注意：`javac` 可以隐式编译一些没有在命令行中提及的源文件。用 `-verbose` 选项可跟踪自动编译。

**文件列表**

<!-- Don't document @files for arguments other than files, such as options - dps <h2>COMMAND LINE ARGUMENT FILE</h2> -->

为缩短或简化 `javac` 命令，可以指定一个或多个每行含有一个文件名的文件。在命令行中，采用 `@` 字符加上文件名的方法将它指定为文件列表。当 `javac` 遇到以 `@` 字符开头的参数时，它对那个文件中所含文件名的操作跟对命令行中文件名的操作是一样的。这使得 Windows 命令行长度不再受限制。

*例如*：

可以在名为 `sourcefiles` 的文件中列出所有源文件的名称。

```
MyClass1.java
MyClass2.java
MyClass3.java
```

然后可用下列命令运行编译器：

```bash
$ javac @sourcefiles
```

**选项**

编译器有一批标准选项，目前的开发环境支持这些标准选项，将来的版本也将支持它。还有一批附加的非标准选项是目前的虚拟机实现所特有的，将来可能要有变化。非标准选项以 `-X` 打头。

**标准选项**

`-classpath` 类路径设置用户类路径，它将覆盖 **CLASSPATH** 环境变量中的用户类路径。若既未指定 **CLASSPATH** 又未指定 `-classpath`，则用户类路径由当前目录构成。有关详细信息，请参阅设置类路径。

若未指定 `-sourcepath` 选项，则将在用户类路径中查找类文件和源文件。

`-d` 目录设置类文件的目标目录。如果某个类是一个包的组成部分，则 `javac` 将把该类文件放入反映包名的子目录中，必要时创建目录。例如，如果指定 `-d c:\myclasses` 并且该类名叫 `com.mypackage.MyClass`，那么类文件就叫作 `c:\myclasses\com\mypackage\MyClass.class`。

若未指定 `-d` 选项，则 `javac` 将把类文件放到与源文件相同的目录中。

注意： `-d` 选项指定的目录不会被自动添加到用户类路径中。

`-deprecation` 显示每种不鼓励使用的成员或类的使用或覆盖的说明。没有给出 `-deprecation` 选项的话，`javac` 将显示这类源文件的名称：这些源文件使用或覆盖不鼓励使用的成员或类。<br>
`-encoding` 设置源文件编码名称，例如 EUCJIS/SJIS。若未指定 `-encoding` 选项，则使用平台缺省的转换器。<br>
`-g` 生成所有的调试信息，包括局部变量。缺省情况下，只生成行号和源文件信息。`-g:none` 不生成任何调试信息。`-g:{关键字列表}` 只生成某些类型的调试信息，这些类型由逗号分隔的关键字列表所指定。有效的关键字有：source 源文件调试信息 lines 行号调试信息 vars 局部变量调试信息 `-nowarn`禁用警告信息。<br>
`-O` 优化代码以缩短执行时间。使用 `-O` 选项可能使编译速度下降、生成更大的类文件并使程序难以调试。

在 JDK 1.2 以前的版本中，`javac` 的 `-g` 选项和 `-O` 选项不能一起使用。在 JDK 1.2 中，可以将 `-g` 和 `-O` 选项结合起来，但可能会得到意想不到的结果，如丢失变量或重新定位代码或丢失代码。`-O` 选项不再自动打开 `-depend` 或关闭 `-g` 选项。同样，`-O` 选项也不再允许进行跨类内嵌。

`-sourcepath` 源路径指定用以查找类或接口定义的源代码路径。与用户类路径一样，源路径项用分号 (;) 进行分隔，它们可以是目录、JAR 归档文件或 ZIP 归档文件。如果使用包，那么目录或归档文件中的本地路径名必须反映包名。

注意：通过类路径查找的类，如果找到了其源文件，则可能会自动被重新编译。

`-verbose` 冗长输出。它包括了每个所加载的类和每个所编译的源文件的有关信息。

**联编选项**

缺省情况下，类是根据与 `javac` 一起发行的 JDK 自举类和扩展类来编译。但 `javac` 也支持联编，在联编中，类是根据其它 Java 平台实现的自举类和扩展类来进行编译的。联编时， `-bootclasspath` 和 `-extdirs` 的使用很重要；请参阅下面的联编程序示例。 

`-target` 版本生成将在指定版本的虚拟机上运行的类文件。缺省情况下生成与 1.1 和 1.2 版本的虚拟机都兼容的类文件。JDK 1.2 中的 `javac` 所支持的版本有：1.1保证所产生的类文件与 1.1 和 1.2 版的虚拟机兼容。这是缺省状态。1.2 生成的类文件可在 1.2 版的虚拟机上运行，但不能在 1.1 版的虚拟机上运行。 <br>
`-bootclasspath` 自举类路径根据指定的自举类集进行联编。和用户类路径一样，自举类路径项用分号 (;) 进行分隔，它们可以是目录、JAR 归档文件或 ZIP 归档文件。<br>
`-extdirs` 目录根据指定的扩展目录进行联编。目录是以分号分隔的目录列表。在指定目录的每个 JAR 归档文件中查找类文件。 <br>

**非标准选项**

`-X` 显示非标准选项的有关信息并退出。<br>
`-Xdepend` 递归地搜索所有可获得的类，以寻找要重编译的最新源文件。该选项将更可靠地查找需要编译的类，但会使编译进程的速度大为减慢。<br>
`-Xstdout` 将编译器信息送到 `System.out` 中。缺省情况下，编译器信息送到 `System.err` 中。<br>
`-Xverbosepath` 说明如何搜索路径和标准扩展以查找源文件和类文件。<br>
`-J` 选项将选项传给 `javac` 调用的 `java` 启动器。例如， `-J-Xms48m` 将启动内存设为 48 兆字节。虽然它不以 `-X` 开头，但它并不是 `javac` 的‘标准选项’。用 `-J` 将选项传给执行用 Java 编写的应用程序的虚拟机是一种公共约定。

注意： **CLASSPATH** 、 `-classpath` 、`-bootclasspath` 和 `-extdirs` 并不指定用于运行 `javac` 的类。如此滥用编译器的实现通常没有任何意义而且总是很危险的。如果确实需要这样做，可用 `-J` 选项将选项传给基本的 `java` 启动器。

**程序示例**

*编译简单程序*

一个源文件 `Hello.java` ，它定义了一个名叫 `greetings.Hello` 的类。greetings 目录是源文件和类文件两者的包目录，且它不是当前目录。这让我们可以使用缺省的用户类路径。它也使我们没必要用 `-d` 选项指定单独的目标目录。

```cmd
C:> dir
greetings/
C:> dir greetings
Hello.java
C:> cat greetings\Hello.java
package greetings;
public class Hello {
    public static void main(String[] args) {
        for (int i=0; i < args.length; i++) {
            System.out.println("Hello " + args[i]);
        }
    }
}
C:> javac greetings\Hello.java
C:> dir greetings
Hello.class   Hello.java
C:> java greetings.Hello World Universe Everyone
Hello World
Hello Universe
Hello Everyone
```

*编译多个源文件*

```cmd
C:> dir
greetings\
C:> dir greetings
Aloha.java         GutenTag.java      Hello.java         Hi.java
C:> javac greetings\*.java
C:> dir greetings
Aloha.class         GutenTag.class      Hello.class         Hi.class
Aloha.java          GutenTag.java       Hello.java          Hi.java
```

*指定用户类路径*

对前面示例中的某个源文件进行更改后，重新编译它：

```cmd
C:> cd
\examples
C:> javac greetings\Hi.java
```

由于 `greetings.Hi` 引用了 greetings 包中其它的类，编译器需要找到这些其它的类。上面的示例能运行是因为缺省的用户类路径刚好是含有包目录的目录。但是，假设我们想重新编译该文件并且不关心我们在哪个目录中的话， 我们需要将 \examples 添加到用户类路径中。可以通过设置 **CLASSPATH** 达到此目的，但这里我们将使用 `-classpath` 选项来完成。

```cmd
C:>javac -classpath \examples \examples\greetings\Hi.java
```

如果再次将 `greetings.Hi` 改为使用标题实用程序，该实用程序也需要通过用户类路径来进行访问：

```cmd
C:>javac -classpath \examples:\lib\Banners.jar \
\examples\greetings\Hi.java
```

要执行 greetings 中的类，需要访问 greetings 和它所使用的类。

```cmd
C:>java -classpath \examples:\lib\Banners.jar greetings.Hi
```

*将源文件和类文件分开*

将源文件和类文件置于不同的目录下经常是很有意义的，特别是在大型的项目中。我们用 `-d` 选项来指明单独的类文件目标位置。由于源文件不在用户类路径中，所以用 `-sourcepath` 选项来协助编译器查找它们。

```cmd
C:> dir
classes\  lib\      src\
C:> dir src
farewells\
C:> dir src\farewells
Base.java      GoodBye.java
C:> dir lib
Banners.jar
C:> dir classes
C:> javac -sourcepath src -classpath classes:lib\Banners.jar \
src\farewells\GoodBye.java -d classes
C:> dir classes
farewells\
C:> dir classes\farewells
Base.class      GoodBye.class
```

注意：编译器也编译了 src\farewells\Base.java，虽然我们没有在命令行中指定它。要跟踪自动编译，可使用 `-verbose` 选项。

*联编程序示例*

这里我们用 JDK 1.2 的 `javac` 来编译将在 1.1 版的虚拟机上运行的代码。

```cmd
C:> javac -target 1.1 -bootclasspath jdk1.1.7\lib\classes.zip \
-extdirs "" OldCode.java
```

JDK 1.2 `javac` 在缺省状态下也将根据 1.2 版的自举类来进行编译，因此我们需要告诉 `javac` 让它根据 JDK 1.1 自举类来进行编译。可用 `-bootclasspath` 和 `-extdirs` 选项来达到此目的。不这样做的话，可能会使编译器根据 1.2 版的 API 来进行编译。由于 1.1 版的虚拟机上可能没有该 1.2 版的 API，因此运行时将出错。

选项可确保生成的类文件与 1.1 版的虚拟机兼容。在 JDK1.2 中， 缺省情况下 `javac` 编译生成的文件是与 1.1 版的虚拟机兼容的，因此并非严格地需要该选项。然而，由于别的编译器可能采用其它的缺省设置，所以提供这一选项将不失为是个好习惯。

[JAVAC 命令详解](https://jeffchen.iteye.com/blog/395671)<br>

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
