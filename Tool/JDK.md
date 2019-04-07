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

> 概要

- `jar c[efmMnv0] [entrypoint] [jarfile] [manifest] [-C dir] file ... [-Joption ...] [@arg-file ...]`

创建 JAR 文件

- `jar u[efmMnv0] [entrypoint] [jarfile] [manifest] [-C dir] file ... [-Joption ...] [@arg-file ...]`

更新 JAR 文件

- `jar x[vf] [jarfile] file ... [-Joption ...] [@arg-file ...]`

提取 JAR 文件

- `jar t[vf] [jarfile] file ... [-Joption ...] [@arg-file ...]`

列出 JAR 文件的内容

- `jar i jarfile [-Joption ...] [@arg-file ...]`

将索引添加到 JAR 文件中

> 描述

`jar` 命令是一个通用的存档和压缩工具，基于 *ZIP* 和 *ZLIB* 压缩格式。但是，`jar` 命令主要用于将 Java applet 或应用程序打包到单个归档文件中。当 applet 或应用程序的组件(文件、图像和声音)被组合到一个单独的归档文件中时，可以由 Java 代理(比如浏览器)在一个单独的 HTTP 事务中下载它们，而不需要为每个部分建立新的连接。这极大地提高了下载时间。`jar` 命令还压缩文件，这进一步提高了下载时间。

`jar` 命令的语法类似于 `tar` 命令的语法。它有几个操作模式，由一个强制操作参数定义。其他参数要么是修改操作行为的选项，要么是执行操作所需的操作数。

> 操作参数

使用 `jar` 命令时，必须通过指定以下操作参数之一来选择要执行的操作。您可以将它们与命令行上的其他单字母选项混合使用，但通常操作参数是指定的第一个参数。

- `c`

创建一个新的 JAR 存档。

- `i`

为 JAR 存档生成索引信息。

- `t`

列出 JAR 归档文件的内容。

- `u`

更新 JAR 存档。

- `x`

从 JAR 归档文件中提取文件。

> 选项

使用以下选项自定义如何创建、更新、提取或查看 JAR 文件:

- `e`

将 *entrypoint* 操作数指定的类设置为绑定到可执行 JAR 文件中的独立 Java 应用程序的入口点。此选项的使用创建或覆盖清单文件中的 `Main-Class` 属性值。当创建(`c`)或更新(`u`) JAR 文件时，可以使用 `e` 选项。

例如，下面的命令使用 *Main.class* 文件创建 *Main.jar* 存档，其中清单中的 `Main-Class` 属性值设置为 `Main`:

```cmd
jar cfe Main.jar Main Main.class
```

如果入口点类名在包中，那么它可以使用点(.)或斜杠(/)作为分隔符。例如，如果 *Main.class* 在一个名为 *mydir* 的包中，则可以通过以下方法之一指定入口点:

```cmd
jar -cfe Main.jar mydir/Main mydir/Main.class
jar -cfe Main.jar mydir.Main mydir/Main.class
```

注意：当一个特定清单还包含 `Main-Class` 属性时，同时指定 `m` 和 `e` 选项，会导致一个模糊的 `Main-Class` 规范。这种模糊性会导致错误，并终止 `jar` 命令的创建或更新操作。

- `f`

设置文件使用 *jarfile* 操作数指定的创建 JAR 文件的名称(`c`)、更新(`u`), (`x`)中提取, 或查看(`t`)省略 `f` 选项并使用 *jarfile* 操作指示 JAR 命令接受来自 stdin 的 JAR 文件的名称(`x` 和 `t`)或将 JAR 文件发送到stdout (`c` 和 `u`)。

- `m`

在 `jar` 命令的清单文件中(位于 *META-INF/MANIFEST.MF* 的归档文件中)包含清单操作数指定的文件中的属性名和值。`jar` 命令将属性的名称和值添加到 `jar` 文件中，除非已经存在具有相同名称的条目，在这种情况下，`jar` 命令将更新属性的值。当创建(`c`)或更新(`u`) JAR 文件时，可以使用 `m` 选项。

可以将特殊用途的名称-值属性对添加到默认清单文件中不包含的清单中。例如，您可以添加指定供应商信息、发布信息、包密封或使 `jar` 绑定的应用程序可执行的属性。有关使用 `m` 选项的示例，请[参见](https://docs.oracle.com/javase/tutorial/deployment/jar/index.html)

- `M`

不创建清单文件条目(针对 `c` 和 `u`)，或者在清单文件条目存在时删除清单文件条目(针对 `u`)。当创建(`c`)或更新(`u`) JAR 文件时，可以使用 `M` 选项。

- `n`

当创建(`c`) JAR 文件时，这个选项将使归档文件规范化，这样内容就不会受到 `pack200`(1)命令的打包和解包操作的影响。如果没有这种规范化，签名 JAR 的签名可能会无效。

- `v`

将详细输出生成为标准输出。

- `0`

(0)在不使用 ZIP 压缩的情况下创建(`c`)或更新(`u`) JAR 文件。

- `-C dir`

当创建(`c`)或更新(`u`) JAR 文件时，此选项在处理文件操作数指定的文件时临时更改目录。它的操作与 `tar` 实用程序的 `-C` 选项类似。例如，下面的命令更改到 *classes* 目录并将 *Bar.class* 文件从该目录添加到 *my.jar*:

```cmd
jar uf my.jar -C classes Bar.class
```

下面的命令将更改到 *classes* 目录并将 *classes* 目录中的所有文件添加到 *my.jar* 中(不需要在 JAR 文件中创建一个类目录)，然后在更改到 *bin* 目录将 *Xyz.class* 添加到 *my.jar* 之前，将更改回原始目录。

```cmd
jar uf my.jar -C classes . -C bin Xyz.class
```

如果类包含文件 *bar1* 和 *bar2*，那么在运行前面的命令之后，JAR 文件将包含以下内容:

```cmd
% jar tf my.jar
META-INF/
META-INF/MANIFEST.MF
bar1
bar2
Xyz.class
```

> 操作对象

`jar` 命令可以识别以下操作数。

- file

当创建(`c`)或更新(`u`) JAR 文件时，文件操作数定义了应该添加到存档中的文件或目录的路径和名称。当提取(`x`)或列出 JAR 文件的内容(`t`)时，文件操作数定义要提取或列出的文件的路径和名称。必须指定至少一个有效的文件或目录。用空格分隔多个文件操作数。如果使用了 *entrypoint*、*jarfile* 或 *manifest* 操作数，则必须在它们之后指定文件操作数。

- entrypoint

当创建(`c`)或更新(`u`) JAR 文件时，*entrypoint* 操作数定义类的名称，该类应该是绑定到可执行 JAR 文件中的独立 Java 应用程序的入口点。如果存在 `e` 选项，则必须指定入口操作数。

- jarfile

定义要创建(`c`)、更新(`u`)、提取(`x`)或查看(`t`)的文件的名称。如果有 `f` 选项，必须指定 *jarfile* 操作数。省略 `f` 选项，*jarfile* 操作数指示 `jar` 命令接受来自 stdin(用于 `x` 和 `t`)的 jar 文件名，或者将 jar 文件发送到 stdout(用于 `c` 和 `u`)。

当索引(`i`) JAR 文件时，指定不带 `f` 选项的 *jarfile* 操作数。

- manifest

当创建(`c`)或更新(`u`) JAR 文件时，*manifest* 操作数用清单中包含的属性的名称和值定义预先存在的清单文件。*MANIFEST.MF* 在 JAR 文件中。如果存在 `f` 选项，则必须指定清单操作数。

## java

> 启动 Java 应用程序。

> 概要

```bash
# options 用空格分隔的命令行选项。
# classname 要启动的类的名称。
# args 传递给 main() 方法的参数由空格分隔。
java [options] classname [args]

# filename 要调用的 Java 归档(JAR)文件的名称。仅与 -jar 选项一起使用。
java [options] -jar filename [args]

javaw [options] classname [args]

javaw [options] -jar filename [args]
```

> 描述

`java` 命令启动 Java 应用程序。它通过启动 Java 运行时环境(JRE)、加载指定的类并调用该类的 *main()* 方法来实现这一点。方法必须声明为 *public* 和 *static*，不能返回任何值，并且必须接受字符串数组作为参数。方法声明的形式如下:

```java
public static void main(String[] args)
```

`java` 命令可以通过加载一个类来启动 JavaFX 应用程序，该类要么具有 *main()* 方法，要么扩展了 javafx.application.Application。在后一种情况下，启动程序 *Application* 类的实例，调用它的 *init()* 方法，然后调用 *start(javafx.stage.Stage)* 方法。

默认情况下，不是 `java` 命令选项的第一个参数是要调用的类的完全限定名。如果指定了 `-jar` 选项，它的参数是包含应用程序的类和资源文件的 JAR 文件的名称。启动类必须由其源代码中的 *Main-Class* 清单头指示。

JRE 在三组位置中搜索 startup 类(以及应用程序使用的其他类): 引导类路径、已安装的扩展名和用户的类路径。

类文件名或 JAR 文件名之后的参数传递给 *main()* 方法。

`javaw` 命令与 `java` 相同，只是 `javaw` 没有相关的控制台窗口。如果不希望出现命令提示窗口，请使用 `javaw`。然而，如果启动失败， `javaw` 启动程序将显示一个包含错误信息的对话框。

> 选项

`java` 命令支持多种选项，可分为以下几类:

- 标准选项
- 非标准选项
- 高级运行时选项
- 高级 JIT 编译选项
- 高级客服务性选项
- 高级垃圾回收选项

Java 虚拟机(JVM)的所有实现都保证支持标准选项。它们用于常见的操作，例如检查 JRE 的版本、设置类路径、启用详细输出等等。

非标准选项是特定于 Java HotSpot 虚拟机的通用选项，因此不能保证所有 JVM 实现都支持它们，它们可能会发生变化。这些选项以 `-X` 开头。

高级选项不推荐用于日常使用。这些是用于调优 Java HotSpot 虚拟机操作的特定区域的开发人员选项，这些操作通常具有特定的系统需求，并且可能需要对系统配置参数的特权访问。它们也不能保证受到所有 JVM 实现的支持，并且可能会发生更改。高级选项从 `-XX` 开始。

为了跟踪在最新版本中被弃用或删除的选项，在文档的末尾有一个名为弃用和删除选项的部分。

布尔选项用于启用默认禁用的特性或禁用默认启用的特性。这些选项不需要参数。Boolean `-XX` 选项使用加号(`-XX:+OptionName`)启用，使用减号(`-XX:-OptionName`)禁用。

对于需要参数的选项，参数可以用空格、冒号(:)或等号(=)与选项名分隔，或者参数可以直接跟随选项(每个选项的确切语法不同)。如果希望以字节为单位指定大小，可以不使用后缀，或者用 *k* 或 *K* 表示千字节(KB)、*m* 或 *M* 表示兆字节(MB)、*g* 或 *G* 表示千兆字节(GB)。例如，要将大小设置为 8 GB，可以指定 *8g*、*8192m*、*8388608k* 或 8589934592 作为参数。如果希望指定百分比，请使用 0 到 1 之间的数字(例如: 0.25 表示 25%)

**标准选项**

这些是 JVM 的所有实现都支持的最常用选项。

- `-agentlib:libname[=options]`

加载指定的本机代理库。在库名称之后，可以使用一个逗号(,)分隔的特定于库的选项列表。

如果指定了选项 `-agentlib:foo`，那么 JVM 将尝试在 *PATH* 系统变量指定的位置加载名为 *foo.dll* 的库。

下面的例子展示了如何加载堆分析工具(HPROF)库，并在堆栈深度为 3 的情况下，每 20 毫秒获取一次示例 CPU 信息:

```bash
-agentlib:hprof=cpu=samples,interval=20,depth=3
```

下面的例子展示了如何加载 Java Debug Wire Protocol (JDWP)库，并侦听端口 8000 上的套接字连接，在主类加载之前挂起 JVM:

```bash
-agentlib:jdwp=transport=dt_socket,server=y,address=8000
```

- `-agentpath:pathname[=options]`

加载由绝对路径名指定的本机代理库。此选项相当于 `-agentlib`，但使用库的完整路径和文件名。

- `-client`

选择 Java HotSpot Client VM。64 位版本的 Java SE 开发工具包(JDK)目前忽略了这个选项，而是使用 Server JVM。

- `-Dproperty=value`

设置系统属性值。属性变量是一个字符串，没有空格表示属性的名称。变量是表示属性值的字符串。如果值是一个带空格的字符串，那么用引号括起来(例如 `-Dfoo="foo bar"`)。

- `-disableassertions[:[packagename]...|:classname]`
- `-da[:[packagename]...|:classname]`

禁用断言。默认情况下，断言在所有包和类中都是禁用的。

如果没有参数，`-disableassertion` (`-da`)将禁用所有包和类中的断言。*packagename* 参数以 *…* 结尾，该开关禁用指定包和任何子包中的断言。如果论点很简单 …，则该开关将禁用当前工作目录中未命名包中的断言。使用 *classname* 参数，将禁用指定类中的断言。

`-disableassertion` (`-da`)选项适用于所有类加载器和系统类(它们没有类加载器)。这个规则有一个例外: 如果这个选项没有提供参数，那么它就不适用于系统类。这使得在除系统类之外的所有类中禁用断言变得很容易。`-disablesystemassertion` 选项允许您在所有系统类中禁用断言。

要显式地启用特定包或类中的断言，请使用 `-enableassertion` (`-ea`)选项。这两个选项可以同时使用。例如，要在 *com.wombat.fruitbat* （和任何子包）中使用断言，但在 *com.wombat.fruitbat.Brickbat* 类中禁用断言，可以使用以下命令运行 *MyClass* 应用程序:

```bash
java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
```

- `-disablesystemassertions`
- `-dsa`

在所有系统类中禁用断言。

- `-enableassertions[:[packagename]...|:classname]`
- `-ea[:[packagename]...|:classname]`

启用断言。默认情况下，断言在所有包和类中都是禁用的。

在没有参数的情况下，`-enableassertion` (`-ea`)在所有包和类中启用断言。*packagename* 参数以 *…* 结尾，该开关启用指定包和任何子包中的断言。如果参数只是 *…*，然后该开关启用当前工作目录中未命名包中的断言。使用 *classname* 参数，选择支持指定类中的断言。

`-enableassertion` (`-ea`)选项适用于所有类加载器和系统类(它们没有类加载器)。这个规则有一个例外: 如果这个选项没有提供参数，那么它就不适用于系统类。这使得在除系统类之外的所有类中启用断言变得很容易。`-enablesystemassertion` 选项提供了一个单独的开关来支持所有系统类中的断言。

要显式禁用特定包或类中的断言，请使用 `-disableassertion` (`-da`)选项。如果一个命令包含这些开关的多个实例，则在加载任何类之前按顺序处理它们。例如，要运行 *MyClass* 应用程序，断言仅在 *com.wombat.fruitbat* 包(以及任何子包)中启用，但在 *com.wombat.fruitbat.Brickbat* 类中禁用，可以使用以下命令:

```bash
java -ea:com.wombat.fruitbat... -da:com.wombat.fruitbat.Brickbat MyClass
```

- `-enablesystemassertions`
- `-esa`

在所有系统类中启用断言。

- `-help`
- `-?`

在不实际运行 JVM 的情况下显示 `java` 命令的使用信息。

- `-jar filename`

执行封装在 JAR 文件中的程序。*filename* 参数是一个 JAR 文件的名称，它的清单包含表单 *Main-Class: classname* 中的一行，该行使用 `public static void main(String[] args)` 方法定义类，该方法作为应用程序的起点。

当您使用 `-jar` 选项时，指定的 JAR 文件是所有用户类的源，其他类路径设置将被忽略。

有关 JAR 文件的更多信息，请参阅以下参考资料:

[jar](#jar)

- `-javaagent:jarpath[=options]`

加载指定的 Java 编程语言代理。

- `-jre-restrict-search`

在版本搜索中包含用户私有的 JREs。

- `-no-jre-restrict-search`

从版本搜索中排除用户私有的 JREs。

- `-server`

选择 Java HotSpot Server VM。JDK 的 64 位版本只支持 Server VM，所以在这种情况下，该选项是隐式的。

- `-showversion`

显示版本信息并继续执行应用程序。这个选项相当于 `-version` 选项，但后者指示 JVM 在显示版本信息后退出。

- `-splash:imgname`

显示带有 *imgname* 指定的图像的启动屏幕。例如，要在启动应用程序时显示 *images* 目录中的 *splash.gif* 文件，请使用以下选项:

```bash
-splash:images/splash.gif
```

- `-verbose:class`

显示每个已加载类的信息。

- `-verbose:gc`

显示关于每个垃圾收集(GC)事件的信息。

- `-verbose:jni`

显示关于使用本机方法和其他 Java 本机接口(JNI)活动的信息。

- `-version`

显示版本信息，然后退出。这个选项相当于 `-showversion` 选项，但后者不会在显示版本信息后指示 JVM 退出。

- `-version:release`

指定用于运行应用程序的发布版本。如果所调用的 `java` 命令版本不符合此规范，并且在系统上找到了适当的实现，那么将使用适当的实现。

*release* 参数指定确切的版本字符串，或者由空格分隔的版本字符串和范围列表。版本字符串是开发人员指定的版本号，格式如下: *1.x.0_u* (其中 *x* 是主版本号，*u* 是更新版本号)。版本范围由一个版本字符串后跟一个加号(+)来指定此版本或更高版本，或者版本字符串的一部分后跟星号(*)来指定具有匹配前缀的任何版本字符串组成。版本字符串和范围可以使用空格来组合逻辑或组合，或者使用 & 来组合两个版本字符串 / 范围的逻辑和组合。例如，如果运行类或JAR文件需要 JRE 6u13(1.6.0_13)或任何从 6u10 开始的 JRE 6(1.6.0_10)，请指定以下内容:

```
-version:"1.6.0_13 1.6* & 1.6.0_10+"
```

只有在 *release* 参数中有空格时，才需要使用引号。

对于 JAR 文件，首选项是在 JAR 文件清单中指定版本需求，而不是在命令行中指定。

**非标准选项**

这些选项是特定于 Java HotSpot 虚拟机的通用选项。

- `-X`

显示所有可用 `-X` 选项的帮助。

- `-Xbatch`

禁用后台编译。默认情况下，JVM 将该方法编译为后台任务，以解释器模式运行该方法，直到后台编译完成。`-Xbatch` 标志禁用后台编译，以便所有方法的编译都作为前台任务进行，直到完成为止。

这个选项相当于 `-XX:-BackgroundCompilation`。

- `-Xbootclasspath:path`

指定以分号(;)分隔的目录、JAR 文件和 ZIP 存档的列表，以搜索引导类文件。这些文件用于替代 JDK 中包含的引导类文件。

不要部署使用此选项覆盖 *rt.jar* 中的类的应用程序，因为这违反了 JRE 二进制代码许可。

- `-Xbootclasspath/a:path`

指定以分号(;)分隔的目录、JAR 文件和 ZIP 存档的列表，以附加到默认引导类路径的末尾。

不要部署使用此选项覆盖 *rt.jar* 中的类的应用程序，因为这违反了 JRE 二进制代码许可。

- `-Xbootclasspath/p:path`

指定以分号(;)分隔的目录、JAR 文件和 ZIP 存档的列表，以将其放在默认引导类路径的前面。

不要部署使用此选项覆盖 *rt.jar* 中的类的应用程序，因为这违反了 JRE 二进制代码许可。

- `-Xcheck:jni`

对 Java 本机接口(JNI)函数执行附加检查。具体来说，它在处理 JNI 请求之前验证传递给 JNI 函数的参数和运行时环境数据。遇到的任何无效数据都表明本机代码中存在问题，在这种情况下，JVM 将以不可恢复的错误终止。当使用此选项时，预计会出现性能下降。

- `-Xcomp`

强制在第一次调用时编译方法。默认情况下，Client VM (`-client`)执行 1,000 个解释方法调用，而 Server VM (`-server`)执行 10,000 个解释方法调用，以收集信息以便高效编译。指定 `-Xcomp` 选项禁用解释方法调用，以牺牲效率提高编译性能。

- `-Xdebug`

什么也不做。提供向后兼容性。

- `-Xdiag`

显示其他诊断消息。

- `-Xfuture`

启用严格的类文件格式检查，以强制严格符合类文件格式规范。鼓励开发人员在开发新代码时使用此标志，因为在将来的版本中，更严格的检查将成为默认设置。

- `-Xint`

以纯解释模式运行应用程序。禁用编译到本机代码，所有字节码由解释器执行。在这种模式下，just in time (JIT)编译器所提供的性能优势并不存在。

- `-Xinternalversion`

显示比 `-version` 选项更详细的 JVM 版本信息，然后退出。

- `-Xloggc:filename`

设置应将详细 GC 事件信息重定向到其中以进行日志记录的文件。写入该文件的信息类似于 `-verbose:gc` 的输出，其中包含从每个记录事件之前的第一个 GC 事件开始所经过的时间。如果使用相同的 `java` 命令为 `-Xloggc` 选项覆盖 `-verbose:gc`。

```bash
-Xloggc:garbage-collection.log
```

- `-Xmaxjitcodesize=size`

指定 JIT 编译代码的最大代码缓存大小(以字节为单位)。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认的最大代码缓存大小为 240 MB;如果使用 `-XX:-TieredCompilation` 选项禁用分层编译，那么默认大小为 48 MB:

```
-Xmaxjitcodesize=240m
```

这个选项相当于 `-XX:ReservedCodeCacheSize`。

- `-Xmixed`

由解释器执行除热方法外的所有字节码，热方法编译为本机代码。

- `-Xmnsize`

为年轻代(托儿所)设置堆的初始大小和最大大小(以字节为单位)。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。

堆的年轻代区域用于新对象。GC 在这个区域执行的频率比在其他区域高。如果年轻一代的大小太小，那么将执行大量较小的垃圾收集。如果大小过大，则只执行完整的垃圾收集，这可能需要很长时间才能完成。Oracle 建议将年轻一代的堆大小保持在总堆大小的一半到四分之一之间。

下面的例子展示了如何使用不同的单元将年轻一代的初始和最大大小设置为 256 MB:

```
-Xmn256m
-Xmn262144k
-Xmn268435456
```

您可以使用 `-XX:NewSize` 来设置初始大小，并使用 `-XX:MaxNewSize` 来设置最大大小，而不是使用 `-Xmn` 选项来为年轻一代设置堆的初始大小和最大大小。

- `-Xmssize`

设置堆的初始大小(以字节为单位)。这个值必须是 1024 的倍数，大于 1 MB。在后面加上字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示千兆字节。

下面的例子演示了如何使用不同的单元将分配的内存大小设置为 6 MB:

```
-Xms6291456
-Xms6144k
-Xms6m
```

如果不设置此选项，则初始大小将设置为分配给老一代和年轻一代的大小之和。可以使用 `-Xmn` 选项或 `-XX:NewSize` 选项设置年轻代的堆的初始大小。

- `-Xmxsize`

指定内存分配池的最大大小(以字节为单位)。这个值必须是 1024 的倍数，大于 2 MB。在后面加上字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示千兆字节。默认值是在运行时根据系统配置选择的。对于服务器部署，`-Xms` 和 `-Xmx` 通常设置为相同的值。

下面的例子演示了如何使用不同的单元将分配内存的最大允许大小设置为 80 MB:

```
-Xmx83886080
-Xmx81920k
-Xmx80m
```

`-Xmx` 选项相当于 `-XX:MaxHeapSize`

- `-Xnoclassgc`

禁用类的垃圾收集(GC)。这可以节省一些 GC 时间，从而缩短应用程序运行期间的中断。

当您在启动时指定 `-Xnoclassgc` 时，应用程序中的类对象将在 GC 期间保持不变，并且始终被认为是活动的。这可能导致更多内存被永久占用，如果不小心使用，就会抛出 out of memory 异常。

- `-Xprof`

配置正在运行的程序，并将配置数据发送到标准输出。此选项作为实用程序提供，在程序开发中很有用，但不打算在生产系统中使用。

- `-Xrs`

减少 JVM 对操作系统信号的使用。

关闭挂钩通过在关闭时运行用户清理代码(例如关闭数据库连接)，使 Java 应用程序能够有序地关闭，即使 JVM 突然终止。

JVM 监视控制台控制事件，以实现用于意外终止的关机挂钩。具体地说，JVM 注册一个控制台控制处理程序，该程序开始关闭钩子处理，并为CTRL_C_EVENT、CTRL_CLOSE_EVENT、CTRL_LOGOFF_EVENT 和 CTRL_SHUTDOWN_EVENT 返回 TRUE。

JVM 使用类似的机制来实现转储线程堆栈的特性，以便进行调试。JVM 使用 CTRL_BREAK_EVENT 执行线程转储。

如果 JVM 作为服务运行(例如，作为 web 服务器的 servlet 引擎)，那么它可以接收 CTRL_LOGOFF_EVENT，但不应该启动关机，因为操作系统实际上不会终止进程。为了避免这种可能的干扰，可以使用 `-Xrs` 选项。当使用 `-Xrs` 选项时，JVM 不会安装控制台控制处理程序，这意味着它不会监视或处理 `CTRL_C_EVENT`、`CTRL_CLOSE_EVENT`、`CTRL_LOGOFF_EVENT` 或 `CTRL_SHUTDOWN_EVENT`。

指定 `-Xrs` 有两个结果:

Ctrl + Break 线程转储不可用。

用户代码负责在 JVM 终止时调用 System.exit() 来运行关闭挂钩。

- `-Xshare:mode`

设置类数据共享(CDS)模式。此选项的可能模式参数包括:

*auto*: 如果可能，请使用 CDS。这是 Java HotSpot 32 位客户端 VM 的默认值。
*on*: 要求使用光盘。如果无法使用类数据共享，则打印错误消息并退出。
*off*: 不要使用 CDS。这是 Java HotSpot 32 位 Server VM、Java HotSpot 64 位 Client VM 和 Java HotSpot 64 位 Server VM 的默认值。
*dump*: 手动生成 CDS 存档。按照“设置类路径”中的描述指定应用程序类路径。您应该在每次新的 JDK 发行版中重新生成 CDS 存档。

- `-XshowSettings:category`

显示设置并继续。这个选项可能的类别参数包括:

*all*: 显示所有类别的设置。这是默认值。
*locale*: 显示与区域设置相关的设置。
*properties*: 显示与系统属性相关的设置。
*vm*: 显示 JVM 的设置。

- `-Xsssize`

设置线程堆栈大小(以字节为单位)。添加字母 *k* 或 *K* 表示 KB, *m* 或 *M* 表示 MB, *g* 或 *G* 表示 GB。默认值取决于虚拟内存。

下面的例子将不同单位的线程堆栈大小设置为 1024 KB:

```
-Xss1m
-Xss1024k
-Xss1048576
```

这个选项相当于 `-XX:ThreadStackSize`。

- `-Xverify:mode`

设置字节码验证器的模式。

不要关闭验证，因为这降低了 Java 提供的保护，并且可能由于格式不正确的类文件而导致问题。

此选项的可能 *mode* 参数包括:

*remote*: 验证所有未由引导程序类加载器加载的字节码。如果没有指定 `-Xverify` 选项，这是默认行为。
*all*: 启用对所有字节码的验证。
*none*: 禁用所有字节码的验证。不推荐使用 `-Xverify:none`。

**高级运行时选项**

这些选项控制 Java HotSpot VM 的运行时行为。

- `-XX:+CheckEndorsedAndExtDirs`

如果 `java` 命令使用认可的标准覆盖机制或扩展机制，则启用此选项以防止 `java` 命令运行 Java 应用程序。此选项检查应用程序是否使用这些机制之一，方法如下:

*java.ext.dirs* 或者 *java.endorsed.dirs* 系统属性被设置

存在 *lib/endorsed* 目录且不是空的

*lib/ext* 目录包含除了 JDK 之外的任何 JAR 文件

system-wide-platform-specific 扩展目录包含任何 JAR 文件

- `-XX:+DisableAttachMechanism`

启用禁用允许工具附加到 JVM 的机制的选项。默认情况下，这个选项是禁用的，这意味着启用了附加机制，您可以使用诸如 `jcmd`、`jstack`、`jmap` 和 `jinfo` 之类的工具。

- `-XX:ErrorFile=filename`

指定发生不可恢复错误时写入错误数据的路径和文件名。默认情况下，这个文件是在当前工作目录中创建的，名为 *hs_err_pidpy.log*，其中 *pid* 是导致错误的进程的标识符。下面的例子展示了如何设置默认日志文件(注意，进程的标识符指定为 %p):

```
-XX:ErrorFile=./hs_err_pid%p.log
```

下面的示例显示如何将错误日志文件设置为 *C:/log/java/java_error.log*:

```
-XX:ErrorFile=C:/log/java/java_error.log
```

如果无法在指定的目录中创建文件(由于空间不足、权限问题或其他问题)，则在操作系统的临时目录中创建文件。临时目录由 *TMP* 环境变量的值指定;如果没有定义该环境变量，则使用 *TEMP* 环境变量的值。

- `-XX:+FailOverToOldVerifier`

当新类型检查程序失败时，启用自动故障转移到旧验证程序。默认情况下，这个选项是禁用的，对于具有最新字节码版本的类，它将被忽略(即被视为禁用)。您可以为使用字节码的旧版本的类启用它。

- `-XX:+FlightRecorder`

启用在应用程序运行时使用 Java 飞行记录器(JFR)。这是一个与 `-XX:+ UnlockCommercialFeatures` 选项一起工作的**商业特性**，如下所示:

```bash
java -XX:+UnlockCommercialFeatures -XX:+FlightRecorder
```

如果没有提供此选项，则仍然可以在运行的 JVM 中通过提供适当的 `jcmd` 诊断命令启用 Java Flight Recorder。

- `-XX:-FlightRecorder`

在应用程序运行时禁用 Java 飞行记录器(JFR)。这是一个与 `-XX:+ UnlockCommercialFeatures` 选项一起工作的**商业特性**，如下所示:

```bash
java -XX:+UnlockCommercialFeatures -XX:-FlightRecorder
```

如果提供此选项，则无法在运行的 JVM 中启用 Java 飞行记录器。

- `-XX:FlightRecorderOptions=parameter=value`

设置控制 JFR 行为的参数。这是一个与 `-XX:+UnlockCommercialFeatures` 选项一起工作的**商业特性**。此选项只能在启用 JFR 时使用(即指定 `-XX:+FlightRecorder` 选项)。

以下列表包含所有可用的 JFR 参数:

*defaultrecording={true|false}*: 指定记录是连续的背景记录，还是在有限的时间内运行。默认情况下，该参数被设置为 *false*(记录运行有限的时间)。若要使记录连续运行，请将参数设置为 *true*。

*disk={true|false}*: 指定 JFR 是否应该将连续记录写入磁盘。默认情况下，该参数被设置为 *false*(禁止对磁盘进行连续记录)。要启用它，请将参数设置为 *true*，并设置 *defaultrecording=true*。

*dumponexit={true|false}*: 指定当 JVM 以受控方式终止时，是否应该生成 JFR 数据的转储文件。默认情况下，该参数设置为 *false*(不会生成退出时的转储文件)。要启用它，请将参数设置为 *true*，并设置 *defaultrecording=true*。如果指定的路径是目录，JVM 将分配一个文件名，该文件名显示创建日期和时间。如果指定的路径包含文件名，并且该文件已经存在，JVM 将通过将日期和时间戳附加到指定的文件名来创建一个新文件。

*globalbuffersize=size*: 指定用于数据保留的主内存总量(以字节为单位)。追加 *k* 或 *K*，以 KB 指定大小，*m* 或 *M* 以 MB 指定大小，*g* 或 *G* 以 GB 指定大小。默认情况下，大小设置为 462848 字节。

*loglevel={quiet|error|warning|info|debug|trace}*: 指定 JFR 写入日志文件的数据量。默认情况下，它被设置为 *info*。

*maxage=time*: 指定为默认记录保留的磁盘数据的最大年龄。附加 *s* 以秒为单位指定时间，*m* 表示分钟，*h* 表示小时，*d* 表示天(例如，指定 30s 表示 30 秒)。默认情况下，最大年龄设置为 15 分钟(15m)。

*maxchunksize=size*: 指定记录中数据块的最大大小(以字节为单位)。追加 *k* 或 *K*，以 KB 指定大小，*m* 或 *M* 以 MB 指定大小，*g* 或 *G* 以 GB 指定大小。默认情况下，数据块的最大大小设置为 12 MB。

*maxsize=size*: 指定为默认记录保留的磁盘数据的最大大小(以字节为单位)。追加 *k* 或 *K*，以 KB 指定大小，*m* 或 *M* 以 MB 指定大小，*g* 或 *G* 以 GB 指定大小。默认情况下，磁盘数据的最大大小不受限制，该参数设置为 0。只有在设置 *disk=true* 参数时，此参数才有效。

*repository=path*: 指定用于临时磁盘存储的存储库(目录)。默认情况下，使用系统的临时目录。

*samplethreads={true|false}*: 指定是否启用线程抽样。只有在启用了抽样事件和该参数时才会发生线程抽样。默认情况下，启用此参数。

*settings=path*: 指定事件设置文件的路径和名称(类型为 JFC)。默认情况下，使用 *default.jfc* 文件，该文件位于 *JAVA_HOME/jre/lib/jfr* 中。

*stackdepth=depth*: 堆栈深度的堆栈跟踪由 JFR。默认情况下，深度被设置为 64 个方法调用。最大值是 2048，最小值是 1。

*threadbuffersize=size*: 指定每个线程的本地缓冲区大小(以字节为单位)。追加 *k* 或 *K*，以 KB 指定大小，*m* 或 *M* 以 MB 指定大小，*g* 或 *G* 以 GB 指定大小。该参数的值越高，就可以在不争用的情况下收集更多的数据，从而将其刷新到全局存储中。它可以在线程丰富的环境中增加应用程序占用空间。默认情况下，本地缓冲区大小设置为 5 KB。

可以用逗号分隔多个参数，从而为它们指定值。例如，要指示 JFR 将连续记录写入磁盘，并将数据块的最大大小设置为 10 MB，请指定以下内容:

```
-XX:FlightRecorderOptions=defaultrecording=true,disk=true,maxchunksize=10M
```

- `-XX:LargePageSizeInBytes=size`

在 Solaris 上，为用于 Java 堆的大页面设置最大大小(以字节为单位)。 *size* 参数必须是 2(2,4,8,16，…)的幂。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认情况下，大小设置为 0，这意味着 JVM 自动为大页面选择大小。

下面的示例演示如何将大页面大小设置为 4 MB:

```
-XX:LargePageSizeInBytes=4m
```

- `-XX:MaxDirectMemorySize=size`

设置新 I/O (java.nio)的最大总大小(以字节为单位)直接缓冲区分配。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认情况下，大小设置为 0，这意味着 JVM 自动选择 NIO 直接缓冲区分配的大小。

以下示例演示如何在不同的单元中将 NIO 大小设置为 1024 KB:

```
-XX:MaxDirectMemorySize=1m
-XX:MaxDirectMemorySize=1024k
-XX:MaxDirectMemorySize=1048576
```

- `-XX:NativeMemoryTracking=mode`

指定跟踪 JVM 本机内存使用情况的模式。此选项的可能模式参数包括:

*off*: 不要跟踪JVM本机内存的使用情况。如果不指定 `-XX:NativeMemoryTracking` 选项，这是默认行为。

*summary*: 只跟踪 JVM 子系统(如 Java 堆、类、代码和线程)的内存使用情况。

*detail*: 除了跟踪 JVM 子系统的内存使用情况外，还跟踪各个 *CallSite*、各个虚拟内存区域及其提交区域的内存使用情况。

- `-XX:ObjectAlignmentInBytes=alignment`

设置 Java 对象的内存对齐(以字节为单位)。默认情况下，该值设置为 8 字节。指定的值应该是 2 的幂，并且必须在 8 和 256(包括)的范围内。此选项使使用具有较大 Java 堆大小的压缩指针成为可能。

堆大小限制(以字节为单位)计算为:

```
4GB * ObjectAlignmentInBytes
```

注意: 随着对齐值的增加，对象之间未使用的空间也会增加。因此，您可能没有意识到使用具有较大 Java 堆大小的压缩指针有任何好处。

- `-XX:OnError=string`

设置发生不可恢复错误时要运行的自定义命令或一系列分号分隔的命令。如果字符串包含空格，则必须用引号括起来。

下面的例子展示了如何使用 `-XX:OnError` 选项来运行 *userdump.exe* 实用程序，在出现不可恢复错误时获得崩溃转储(%p 指定当前进程):

```
-XX:OnError="userdump.exe %p"
```

前面的示例假设 *userdump.exe* 实用程序的路径是在 *PATH* 环境变量中指定的。

- `-XX:OnOutOfMemoryError=string`

设置在首次抛出 OutOfMemoryError 异常时要运行的自定义命令或一系列分号分隔的命令。如果字符串包含空格，则必须用引号括起来。有关命令字符串的示例，请参见 `-XX:OnError` 选项的说明。

- `-XX:+PerfDataSaveToFile`

如果启用，则在 Java 应用程序退出时保存 `jstat` 二进制数据。这个二进制数据保存在一个名为 `hsperfdata_<pid>` 的文件中，其中 `<pid>` 是您运行的 Java 应用程序的进程标识符。使用 `jstat` 显示该文件中包含的性能数据如下:

```
jstat -class file:///<path>/hsperfdata_<pid>
jstat -gc file:///<path>/hsperfdata_<pid>
```

- `-XX:+PrintCommandLineFlags`

允许打印出现在命令行上的符合人体工程学的选择的 JVM 标志。了解 JVM 设置的符合人体工程学的值(比如堆空间大小和所选的垃圾收集器)可能会很有用。默认情况下，禁用此选项，并且不打印标志。

- `-XX:+PrintNMTStatistics`

在启用本机内存跟踪时，启用在 JVM 出口打印收集的本机内存跟踪数据(请参见 `-XX:NativeMemoryTracking`)。默认情况下，禁用此选项，并且不打印本机内存跟踪数据。

- `-XX:+RelaxAccessControlCheck`

减少验证程序中的访问控制检查量。默认情况下，这个选项是禁用的，对于具有最新字节码版本的类，它将被忽略(即被视为禁用)。您可以为使用字节码的旧版本的类启用它。

- `-XX:+ResourceManagement`

启用在应用程序运行时使用资源管理。

这是一个**商业特性**，需要您还指定 `-XX:+UnlockCommercialFeatures` 选项，如下所示:

```bash
java -XX:+UnlockCommercialFeatures -XX:+ResourceManagement
```

- `-XX:ResourceManagementSampleInterval=value (milliseconds)`

设置控制资源管理度量的采样间隔的参数，以毫秒为单位。

此选项只能在启用资源管理时使用(即指定 `-XX:+ResourcemanManagement` 选项)。

- `-XX:SharedArchiveFile=path`

指定类数据共享(CDS)存档文件的路径和名称

- `-XX:SharedClassListFile=file_name`

指定包含要存储在类数据共享(CDS)存档中的类文件的名称的文本文件。这个文件每行包含一个类文件的全名，使用斜杠(/)替换点(.)。例如，指定 *java.lang.Object* 和 *hello.Main*。创建一个包含以下两行内容的文本文件:

```
java/lang/Object
hello/Main
```

在此文本文件中指定的类文件应该包含应用程序通常使用的类。它们可能包含应用程序、扩展或引导类路径中的任何类。

- `-XX:+ShowMessageBoxOnError`

当 JVM 遇到不可恢复的错误时，启用显示对话框。这可以防止 JVM 退出并保持进程处于活动状态，以便您可以将调试器附加到进程上，以调查错误的原因。默认情况下，此选项是禁用的。

- `-XX:StartFlightRecording=parameter=value`

启动 Java 应用程序的 JFR 记录。这是一个与 `-XX:+UnlockCommercialFeatures` 选项一起工作的**商业特性**。这个选项相当于 JFR。启动在运行时启动记录的诊断命令。在开始录制 JFR 时，可以设置以下参数:

*compress={true|false}*: 指定是否使用 *gzip* 文件压缩实用程序压缩磁盘上的 JFR 记录日志文件(类型为 JFR)。此参数仅在指定文件名参数时有效。默认情况下，它被设置为 *false*(记录没有被压缩)。若要启用压缩，请将参数设置为 *true*。

*defaultrecording={true|false}*: 指定记录是连续的背景记录，还是在有限的时间内运行。默认情况下，该参数被设置为 *false* (记录运行有限的时间)。若要使记录连续运行，请将参数设置为 *true*。

*delay=time*: 指定 Java 应用程序启动时间与记录开始之间的延迟。附加 *s* 以秒为单位指定时间，*m* 表示分钟，*h* 表示小时，*d* 表示天(例如，指定 10m 表示 10 分钟)。默认情况下，没有延迟，并且该参数被设置为 0。

*dumponexit={true|false}*: 指定当 JVM 以受控方式终止时，是否应该生成 JFR 数据的转储文件。默认情况下，该参数设置为 *false*(不会生成退出时的转储文件)。要启用它，请将参数设置为 *true*。转储文件被写入 *filename* 参数定义的位置。

```
-XX:StartFlightRecording=name=test,filename=D:\test.jfr,dumponexit=true
```

*duration=time*: 指定记录的持续时间。附加 *s* 以秒为单位指定时间，*m* 表示分钟，*h* 表示小时，*d* 表示天(例如，指定 5h 表示 5 小时)。默认情况下，持续时间不受限制，该参数设置为 0。

*filename=path*: 指定 JFR 记录日志文件的路径和名称。

*name=identifier*: 指定 JFR 记录的标识符。默认情况下，它被设置为 *Recording x*。

*maxage=time*: 指定为默认记录保留的磁盘数据的最大年龄。附加 *s* 以秒为单位指定时间，*m* 表示分钟，*h* 表示小时，*d* 表示天(例如，指定 30s 表示 30 秒)。默认情况下，最大年龄设置为 15 分钟(15m)。

*maxsize=size*: 指定为默认记录保留的磁盘数据的最大大小(以字节为单位)。追加 *k* 或 *K*，以 KB 指定大小，*m* 或 *M* 以 MB 指定大小，*g* 或 *G* 以 GB 指定大小。默认情况下，磁盘数据的最大大小不受限制，该参数设置为 0。

*settings=path*: 指定事件设置文件的路径和名称(类型为JFC)。默认情况下，使用 *default.jfc* 文件，该文件位于 *JAVA_HOME/jre/lib/jfr* 中。

可以用逗号分隔多个参数，从而为它们指定值。例如，保存要测试的记录。jfr在当前工作目录下，并指示 JFR 压缩日志文件，具体如下:

```
-XX:StartFlightRecording=filename=test.jfr,compress=true
```

- `-XX:ThreadStackSize=size`

设置线程堆栈大小(以字节为单位)。添加字母 *k* 或 *G* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认值取决于虚拟内存。

下面的例子演示了如何在不同的单元中将线程堆栈大小设置为 1024 KB:

```
-XX:ThreadStackSize=1m
-XX:ThreadStackSize=1024k
-XX:ThreadStackSize=1048576
```

这个选项等价于 `-Xss`。

- `-XX:+TraceClassLoading`

启用类加载时的跟踪。默认情况下，禁用此选项，并且不跟踪类。

- `-XX:+TraceClassLoadingPreorder`

启用对所有加载的类的跟踪，以引用它们的顺序进行跟踪。默认情况下，禁用此选项，并且不跟踪类。

- `-XX:+TraceClassResolution`

启用对恒定池分辨率的跟踪。默认情况下，禁用此选项，并且不跟踪常量池解析。

- `-XX:+TraceClassUnloading`

启用在类卸载时跟踪它们。默认情况下，禁用此选项，并且不跟踪类。

- `-XX:+TraceLoaderConstraints`

启用跟踪加载器约束记录。默认情况下，禁用此选项，并且不跟踪加载器约束记录。

- `-XX:+UnlockCommercialFeatures`

支持使用商业特性。

默认情况下，这个选项是禁用的，JVM 运行时没有商业特性。一旦为 JVM 进程启用了它们，就不可能禁用它们对该进程的使用。

如果不提供此选项，则仍然可以使用适当的 `jcmd` 诊断命令在运行的 JVM 中解锁商业特性。

- `-XX:+UseAppCDS`

启用应用程序类数据共享(AppCDS)。要使用 AppCDS，还必须在 CDS 转储时间(请参阅选项 `-Xshare:dump`)和应用程序运行时为选项 `-XX:SharedClassListFile` 和 `-XX:SharedArchiveFile` 指定值。

这是一个**商业特性**，需要您还指定 `-XX:+UnlockCommercialFeatures` 选项。这也是一个实验特性;它可能会在未来的版本中发生变化。

- `-XX:-UseBiasedLocking`

禁用偏置锁定的使用。启用此标志后，具有大量非争用同步的应用程序可能会获得显著的速度提升，而具有特定锁定模式的应用程序可能会出现速度下降。有关偏向锁定技术的更多信息，请参见 Java 调优白皮书中的示例: http://www.oracle.com/technetwork/java/tuning-139912.html#section4.2.5

默认情况下，启用此选项。

- `-XX:-UseCompressedOops`

禁用压缩指针的使用。默认情况下，这个选项是启用的，当 Java 堆大小小于 32 GB 时，将使用压缩指针。当启用此选项时，对象引用被表示为 32 位偏移量，而不是 64 位指针，当运行 Java 堆大小小于 32 GB的应用程序时，这通常会提高性能。此选项仅适用于 64 位 JVMs。

当 Java 堆大小大于 32GB时，也可以使用压缩指针。请参见 `-XX:ObjectAlignmentInBytes` 选项。

- `-XX:+UseLargePages`

启用大页内存的使用。默认情况下，禁用此选项，不使用大页面内存。

- `-XX:+UseMembar`

允许在线程状态转换时发出 membar。默认情况下，除 ARM 服务器之外的所有平台都禁用此选项，而 ARM 服务器是启用此选项的。(建议您不要在 ARM 服务器上禁用此选项。)

- `-XX:+UsePerfData`

启用 *perfdata* 特性。默认情况下启用此选项，以允许 JVM 监视和性能测试。禁用它将抑制 *hsperfdata_userid* 目录的创建。要禁用 *perfdata* 特性，请指定 `-XX:-UsePerfData`。

- `-XX:+AllowUserSignalHandlers`

允许应用程序安装信号处理程序。默认情况下，此选项是禁用的，应用程序不允许安装信号处理程序。

**高级 JIT 编译选项**

这些选项控制 Java HotSpot VM 执行的动态即时(JIT)编译。

- `-XX:+AggressiveOpts`

启用积极的性能优化特性，这些特性预计将在即将发布的版本中成为默认特性。默认情况下，此选项是禁用的，不使用实验性性能特性。

- `-XX:AllocateInstancePrefetchLines=lines`

设置要在实例分配指针之前预取的行数。默认情况下，预取的行数设置为 1:

```
-XX:AllocateInstancePrefetchLines=1
```

只有 Java HotSpot Server VM 支持该选项。

- `-XX:AllocatePrefetchDistance=size`

设置对象分配的预取距离的大小(以字节为单位)。将使用新对象的值写入的内存将从最后分配对象的地址开始预取到这个距离。每个 Java 线程都有自己的分配点。

负值表示基于平台选择预取距离。正值是要预取的字节。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认值设置为 -1。

下面的例子展示了如何将预取距离设置为 1024 字节:

```
-XX:AllocatePrefetchDistance=1024
```

只有 Java HotSpot Server VM 支持该选项。

- `-XX:AllocatePrefetchInstr=instruction`

将预取指令设置为在分配指针之前预取。只有 Java HotSpot VM 支持此选项。可能的值从 0 到 3。值后面的实际指令取决于平台。默认情况下，prefetch 指令设置为0。

- `-XX:AllocatePrefetchLines=lines`

通过使用编译代码中生成的预取指令，设置在最后一次对象分配之后要加载的高速缓存线路的数量。如果最后分配的对象是实例，则默认值为 1，如果是数组，则默认值为 3。

只有 Java HotSpot Server VM 支持该选项。

- `-XX:AllocatePrefetchStepSize=size`

为顺序预取指令设置步骤大小(以字节为单位)。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。默认情况下，步长设置为16字节。

只有 Java HotSpot Server VM 支持该选项。

- `-XX:AllocatePrefetchStyle=style`

为预取指令设置生成的代码样式。样式参数是从 0 到 3 的整数:

*0*: 不要生成预取指令。

*1*: 每次分配之后执行预取指令。这是默认参数。

*2*: 使用线程本地分配块(TLAB)水印指针来确定何时执行预取指令。

*3*: 使用 SPARC 上的 BIS 指令进行分配预取。

只有 Java HotSpot Server VM 支持该选项。

- `-XX:+BackgroundCompilation`

支持后台编译。默认情况下启用此选项。要禁用后台编译，请指定 `-XX:- BackgroundCompilation`(这相当于指定 `-Xbatch`)。

- `-XX:CICompilerCount=threads`

设置用于编译的编译器线程数。默认情况下，服务器 JVM 的线程数设置为 2，客户机 JVM 的线程数设置为 1，如果使用分层编译，线程数将扩展到内核的数量。

- `-XX:CodeCacheMinimumFreeSpace=size`

设置编译所需的最小空闲空间(以字节为单位)。添加字母 *k* 或 *K* 表示千字节，*m* 或 *M* 表示兆字节，*g* 或 *G* 表示十亿字节。当剩余的可用空间小于最小值时，编译将停止。默认情况下，该选项被设置为 500 KB。

- `-XX:CompileCommand=command,method[,option]`

指定要对方法执行的命令。例如，要将 *String* 类的 *indexOf()* 方法排除在编译之外，可以使用以下方法:

```
-XX:CompileCommand=exclude,java/lang/String.indexOf
```

注意，指定了完整的类名，包括所有用斜杠(/)分隔的包和子包。为了方便剪切和粘贴操作，还可以使用 `-XX:+PrintCompilation` 和 `-XX:+LogCompilation` 选项生成的方法名格式:

```
-XX:CompileCommand=exclude,java.lang.String::indexOf
```

如果指定的方法没有签名，则该命令将应用于所有具有指定名称的方法。但是，您也可以在类文件格式中指定方法的签名。在本例中，应该用引号括住参数，因为否则 shell 将分号作为命令结束。例如，如果您只想在编译时排除 *String* 类的 *indexOf(String)* 方法，请使用以下方法:

```
-XX:CompileCommand="exclude,java/lang/String.indexOf,(Ljava/lang/String;)I"
```

您还可以使用星号(*)作为类和方法名的通配符。例如，要排除所有类中的所有 *indexOf()* 方法，可以使用以下方法:

```
-XX:CompileCommand=exclude,*.indexOf
```

逗号和句点是空格的别名，这使得通过 shell 传递编译器命令更加容易。您可以将参数传递给 `-XX:CompileCommand`，使用空格作为分隔符，将参数括在引号中:

```
-XX:CompileCommand="exclude java/lang/String indexOf"
```

注意，在使用 `-XX:CompileCommand` 选项解析命令行上传递的命令之后，JIT 编译器将从 *.hotspot_compiler* 文件中读取命令。您可以向该文件添加命令，或者使用 `-XX:CompileCommandFile` 选项指定另一个文件。

要添加几个命令，要么多次指定 `-XX:CompileCommand` 选项，要么使用换行分隔符(`\n`)分隔每个参数。以下命令是可用的:

*break*: 在调试 JVM 时设置断点，使其在编译指定方法的开始处停止。

*compileonly*: 排除除指定方法外的所有编译方法。作为替代，您可以使用 `-XX:CompileOnly` 选项，它允许指定几个方法。

*dontinline*: 防止指定方法的内联。

*exclude*: 将指定的方法排除在编译之外。

*help*: 打印 `-XX:CompileCommand` 选项的帮助消息。

*inline*: 尝试内联指定的方法。

*log*: 排除除指定方法之外的所有方法的编译日志记录(使用 `-XX:+LogCompilation` 选项)。默认情况下，对所有已编译的方法执行日志记录。

*option*: 此命令可用于将 JIT 编译选项传递给指定的方法，以替代最后一个参数(选项)。编译选项是在方法名后面的末尾设置的。例如，要为 *StringBuffer* 类的 *append()* 方法启用 *BlockLayoutByFrequency* 选项，请使用以下命令: `-XX:CompileCommand=option,java/lang/StringBuffer.append,BlockLayoutByFrequency` 您可以指定多个编译选项，以逗号或空格分隔。

*print*: 编译指定方法后打印生成的汇编程序代码。

*quiet*: 不要打印编译命令。默认情况下，您使用 `-XX:CompileCommand` 选项指定的命令将被打印出来;例如，如果您在编译 *String* 类的 *indexOf()* 方法时将其排除在外，那么以下内容将被打印到标准输出: `CompilerOracle: exclude java/lang/String.indexOf` 您可以通过在其他 `-XX:CompileCommand` 选项之前指定 `-XX:CompileCommand=quiet` 选项来抑制这种情况。

- `-XX:CompileCommandFile=filename`

设置从其中读取 JIT 编译器命令的文件。默认情况下， *.hotspot_compiler* 文件用于存储 JIT 编译器执行的命令。

命令文件中的每一行都表示一个命令、一个类名和一个使用该命令的方法名。例如，这一行打印 *String* 类的 *toString()* 方法的汇编代码:

```
print java/lang/String toString
```

有关指定 JIT 编译器对方法执行的命令的更多信息，请参见 `-XX:CompileCommand` 选项。

**高级垃圾回收选项**

这些选项控制 Java HotSpot VM 执行垃圾收集(GC)的方式。

- `-XX:+AggressiveHeap`

启用 Java 堆优化。这将根据计算机(RAM 和 CPU)的配置，将各种参数设置为长时间运行的作业的最优参数，这些作业具有密集的内存分配。默认情况下，该选项是禁用的，堆没有优化。

- `-XX:+AlwaysPreTouch`

允许在 JVM 初始化期间触摸 Java 堆上的每个页面。这将在输入 *main()* 方法之前将所有页面都放入内存。该选项可用于测试，以模拟一个长时间运行的系统，其中所有虚拟内存都映射到物理内存。默认情况下，这个选项是禁用的，所有页面都作为 JVM 堆空间的填充提交。

- `-XX:+CMSClassUnloadingEnabled`

启用在使用并发 *标记-清除*(CMS)垃圾收集器时卸载类。默认情况下启用此选项。要禁用 CMS 垃圾收集器的类卸载，请指定 `-XX:-CMSClassUnloadingEnabled`。

- `-XX:CMSExpAvgFactor=percent`

设置用于计算并发收集统计数据的指数平均值时对当前样本加权的时间百分比(0 到 100)。默认情况下，指数平均因子设置为 25%。

- `-XX:CMSInitiatingOccupancyFraction=percent`

设置开始 CMS 收集周期的老一代占用的百分比(0 到 100)。默认值设置为-1。任何负值(包括默认值)都意味着 `-XX:CMSTriggerRatio` 用于定义初始占用率的值。

- `-XX:+CMSScavengeBeforeRemark`

启用 CMS 备注步骤之前的清除尝试。默认情况下，此选项是禁用的。

- `-XX:CMSTriggerRatio=percent`

设置在 CMS 收集周期开始之前分配的 `-XX:MinHeapFreeRatio` 指定的值的百分比(0 到 100)。默认值设置为 80%。

- `-XX:ConcGCThreads=threads`

设置用于并发 GC 的线程数。默认值取决于 JVM 可用 CPU 的数量。

- `-XX:+DisableExplicitGC`

启用禁用对 *System.gc()* 调用的处理的选项。默认情况下禁用此选项，这意味着将处理对 *System.gc()* 的调用。如果禁用了对 *System.gc()* 调用的处理，JVM 仍然在必要时执行 GC。

- `-XX:+ExplicitGCInvokesConcurrent`

通过使用 *System.gc()* 请求启用并发 GC 调用。默认情况下禁用此选项，并且只能与 `-XX:+UseConcMarkSweepGC` 选项一起启用。

- `-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses`

通过使用 *System.gc()* 请求，并在并发 GC 周期中卸载类，支持调用并发 GC。默认情况下禁用此选项，并且只能与 `-XX:+UseConcMarkSweepGC` 选项一起启用。

- `-XX:G1HeapRegionSize=size`

设置使用垃圾优先(G1)收集器时将 Java 堆细分到的区域的大小。值可以在 1 MB到 32 MB 之间。默认区域大小是根据堆大小根据人体工程学确定的。

- `-XX:+G1PrintHeapRegions`

允许打印关于 G1 收集器分配哪些区域和回收哪些区域的信息。默认情况下，此选项是禁用的。

- `-XX:G1ReservePercent=percent`

设置保留为假上限的堆的百分比(0 到 50)，以减少 G1 收集器升级失败的可能性。默认情况下，该选项被设置为 10%。

- `-XX:InitialHeapSize=size`

设置内存分配池的初始大小(以字节为单位)。这个值必须是 0，或者是 1024 的倍数，并且大于 1 MB。默认值是在运行时根据系统配置选择的。

如果将此选项设置为 0，则初始大小将设置为分配给老一代和年轻一代的大小之和。可以使用 `-XX:NewSize` 选项设置年轻代的堆大小。

- `-XX:InitialSurvivorRatio=ratio`

设置吞吐量垃圾收集器使用的初始幸存者空间比率(由 `-XX:+UseParallelGC` 和/或 `-XX:+UseParallelOldGC` 选项启用)。默认情况下，吞吐量垃圾收集器使用 `-XX:+UseParallelGC` 和 `-XX:+UseParallelOldGC` 选项启用自适应大小调整，幸存者空间根据应用程序行为调整大小，从初始值开始。如果禁用了自适应调整大小(使用 `-XX:-UseAdaptiveSizePolicy` 选项)，那么应该使用 `-XX:SurvivorRatio` 选项设置幸存者空间的大小，以便在执行的整个过程中使用.

根据年轻代的规模(Y)和初始幸存者空间比(R)计算幸存者空间的初始大小，公式如下:

```
S=Y/(R+2)
```

方程中的 2 表示两个幸存者空间。指定为初始幸存者空间比的值越大，初始幸存者空间大小就越小。

默认情况下，初始幸存者空间比率设置为 8。如果使用年轻代空间大小的默认值(2 MB)，那么幸存者空间的初始大小将为 0.2 MB。

- `-XX:InitiatingHeapOccupancyPercent=percent`

设置用于启动并发 GC 循环的堆占用百分比(0 到 100)。它由垃圾收集器使用，垃圾收集器根据整个堆的占用情况触发并发 GC 循环，而不只是一个代(例如 G1 垃圾收集器)。

默认情况下，初始值设置为 45%。值 0 表示不停止的 GC 循环。

- `-XX:MaxGCPauseMillis=time`

设置 GC 最大暂停时间的目标(以毫秒为单位)。这是一个软目标，JVM 将尽最大努力实现它。默认情况下，没有最大暂停时间值。

- `-XX:MaxHeapSize=size`

设置内存分配池的最大大小(以 byes 为单位)。这个值必须是 1024 的倍数，大于 2 MB。默认值是在运行时根据系统配置选择的。对于服务器部署， `-XX:InitialHeapSize` 和 `-XX:MaxHeapSize` 通常设置为相同的值。

- `-XX:MaxHeapFreeRatio=percent`

设置 GC 事件后的最大允许空闲堆空间百分比(0 到 100)。如果空闲堆空间扩展到该值之上，则堆将收缩。默认情况下，该值设置为 70%。

- `-XX:MaxMetaspaceSize=size`

设置可分配给类元数据的最大本机内存量。默认情况下，大小没有限制。应用程序的元数据数量取决于应用程序本身、其他正在运行的应用程序以及系统上可用的内存数量。

- `-XX:MaxNewSize=size`

为年轻代(托儿所)设置堆的最大大小(以字节为单位)。默认值是根据人体工程学设置的。

- `-XX:MaxTenuringThreshold=threshold`

设置用于自适应 GC 分级的最大拉伸阈值。最大的值是 15。默认值为并行(吞吐量)收集器为 15,CMS 收集器为 6。

- `-XX:MetaspaceSize=size`

设置分配的类元数据空间的大小，该空间将在第一次超过垃圾收集时触发垃圾收集。垃圾收集的这个阈值根据使用的元数据的数量增加或减少。默认大小取决于平台。

- `-XX:MinHeapFreeRatio=percent`

设置 GC 事件后允许的最小空闲堆空间百分比(0 到 100)。如果空闲堆空间低于此值，则会展开堆。默认情况下，该值设置为 40%。

- `-XX:NewRatio=ratio`

设置年轻一代和年老一代的大小之间的比例。默认情况下，该选项被设置为 2。

- `-XX:NewSize=size`

为年轻代(托儿所)设置堆的初始大小(以字节为单位)。

堆的年轻代区域用于新对象。GC 在这个区域执行的频率比在其他区域高。如果年轻一代的尺寸太小，那么将执行大量的小型 GCs。如果尺寸太大，则只能执行完整的 GCs，这可能需要很长时间才能完成。Oracle 建议将年轻一代的堆大小保持在总堆大小的一半到四分之一之间。

`-XX:NewSize` 与 `-Xmn` 相等。

- `-XX:ParallelGCThreads=threads`

设置年轻代和老代中用于并行垃圾收集的线程数。默认值取决于 JVM 可用 cpu 的数量。

- `-XX:+ParallelRefProcEnabled`

启用并行引用处理。默认情况下，此选项是禁用的。

- `-XX:+PrintAdaptiveSizePolicy`

启用打印有关自适应生成大小的信息。默认情况下，此选项是禁用的。

- `-XX:+PrintGC`

支持在每个 GC 中打印消息。默认情况下，此选项是禁用的。

- `-XX:+PrintGCApplicationConcurrentTime`

启用打印自上次暂停(例如，GC 暂停)以来已经过了多长时间。默认情况下，此选项是禁用的。

- `-XX:+PrintGCApplicationStoppedTime`

启用打印暂停(例如，GC 暂停)持续的时间。默认情况下，此选项是禁用的。

- `-XX:+PrintGCDateStamps`

启用在每个 GC 上打印日期戳。默认情况下，此选项是禁用的。

- `-XX:+PrintGCDetails`

支持在每个 GC 中打印详细消息。默认情况下，此选项是禁用的。

- `-XX:+PrintGCTaskTimeStamps`

启用打印每个 GC 工作线程任务的时间戳。默认情况下，此选项是禁用的。

- `-XX:+PrintGCTimeStamps`

启用在每个 GC 上打印时间戳。默认情况下，此选项是禁用的。

- `-XX:+PrintStringDeduplicationStatistics`

打印详细的重复数据删除统计数据。默认情况下，此选项是禁用的。请参见 `-XX:+UseStringDeduplicatation` 选项。

- `-XX:+PrintTenuringDistribution`

能够打印保存的年龄信息。下面是输出的一个例子:

```
Desired survivor size 48286924 bytes, new threshold 10 (max 10)
- age 1: 28992024 bytes, 28992024 total
- age 2: 1366864 bytes, 30358888 total
- age 3: 1425912 bytes, 31784800 total
...
```

年龄 1 岁的物体是最年轻的幸存者(它们是在前一个拾荒者之后创建的，在最新的拾荒者之后存活下来，并从伊甸园移动到幸存者空间)。年龄 2 岁的物体在两次拾荒中存活(在第二次拾荒中，它们被从一个幸存者空间复制到下一个幸存者空间)。等等。

在前面的示例中，一次清除存活了 28992024 字节，并将其从 eden 复制到幸存者空间，年龄为 2 的对象占用了 1366 864 字节，等等。每行中的第三个值是年龄为 n 或更小的对象的累积大小。

默认情况下，此选项是禁用的。

- `-XX:+ScavengeBeforeFullGC`

启用每个完整 GC 之前的年轻一代的 GC。默认情况下启用此选项。Oracle 建议不要禁用它，因为在完全 GC 之前清除年轻代可以减少从旧代空间到年轻代空间的对象数量。要在每个完整 GC 之前禁用年轻一代的 GC，请指定 `-XX:-ScavengeBeforeFullGC`。

- `-XX:SoftRefLRUPolicyMSPerMB=time`

设置软可达对象在上次引用之后在堆上保持活动的时间量(以毫秒为单位)。默认值是堆中每释放兆字节 1 秒的生命周期。`-XX:SoftRefLRUPolicyMSPerMB` 选项接受整数值，表示当前堆大小(对于Java HotSpot Server VM)或最大堆大小的每兆字节毫秒。这种差异意味着客户机 VM 倾向于刷新软引用而不是增长堆，而服务器 VM 倾向于增长堆而不是增长软引用。在后一种情况下，`-Xmx` 选项的值对垃圾收集软引用的速度有显著影响。

- `-XX:StringDeduplicationAgeThreshold=threshold`

达到指定年龄的字符串对象被认为是重复数据删除的候选对象。对象的年龄是一个度量对象在垃圾收集之后存活了多少次的指标。这有时被称为紧张;请参见 `-XX:+PrintTenuringDistribution` 选项。注意，在达到这个年龄之前提升到旧堆区域的 *String* 对象通常被认为是重复数据删除的候选对象。这个选项的默认值是 3。请参见 `-XX:+UseStringDeduplicatation` 选项。

- `-XX:SurvivorRatio=ratio`

设置伊甸园空间大小与幸存者空间大小之间的比例。默认情况下，该选项被设置为 8。

- `-XX:TargetSurvivorRatio=percent`

设置年轻垃圾收集后使用的幸存者空间的百分比(0 到 100)。默认情况下，该选项被设置为 50%。

- `-XX:TLABSize=size`

设置线程本地分配缓冲区(TLAB)的初始大小(以字节为单位)。

- `-XX:+UseAdaptiveSizePolicy`

启用自适应生成大小调整。默认情况下启用此选项。要禁用自适应生成大小调整，请指定 `-XX:-UseAdaptiveSizePolicy` 并显式设置内存分配池的大小(请参阅 `-XX:SurvivorRatio` 选项)。

- `-XX:+UseCMSInitiatingOccupancyOnly`

启用占用值作为启动 CMS 收集器的唯一标准。默认情况下，此选项是禁用的，可以使用其他条件。

- `-XX:+UseConcMarkSweepGC`

允许使用CMS垃圾收集器对老一代人进行垃圾回收。Oracle 建议在吞吐量(`-XX:+UseParallelGC`)不能满足应用程序延迟需求时使用 CMS 垃圾收集器。G1 垃圾收集器(`-XX:+UseG1GC`)是另一种选择。

默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。当启用此选项时，会自动设置 `-XX:+UseParNewGC` 选项，您不应该禁用它，因为 JDK 8: `-XX:+UseConcMarkSweepGC` `-XX:-UseParNewGC` 中已经禁用了以下选项组合。

- `-XX:+UseG1GC`

启用垃圾优先(G1)垃圾收集器。它是一种服务器风格的垃圾收集器，针对具有大量 RAM 的多处理器机器。它满足 GC 暂停时间目标的概率很高，同时保持良好的吞吐量。G1 收集器推荐用于需要大堆(大小约为 6 GB 或更大)且 GC 延迟要求有限(稳定且可预测的暂停时间低于 0.5 秒)的应用程序。

默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。

- `-XX:+UseGCOverheadLimit`

启用限制 JVM 在抛出 OutOfMemoryError 异常之前花费在 GC 上的时间比例的策略。默认情况下，这个选项是启用的，如果将总时间的 98% 以上花在垃圾收集上，而回收的堆不足 2%，那么并行 GC 将抛出 OutOfMemoryError。当堆很小时，可以使用该特性来防止应用程序长时间运行而很少或没有进展。要禁用此选项，请指定 `-XX:-UseGCOverheadLimit`。

- `-XX:+UseNUMA`

通过增加应用程序对低延迟内存的使用，在具有非均匀内存体系结构(NUMA)的机器上实现应用程序的性能优化。默认情况下，这个选项是禁用的，不会对 NUMA 进行优化。该选项仅在使用并行垃圾收集器时可用(`-XX:+UseParallelGC`)。

- `-XX:+UseParallelGC`

启用并行清除垃圾收集器(也称为吞吐量收集器)，通过利用多个处理器提高应用程序的性能。

默认情况下，这个选项是禁用的，收集器是根据机器的配置和JVM的类型自动选择的。如果启用了它，那么 `-XX:+UseParallelOldGC` 选项将自动启用，除非显式禁用它。

- `-XX:+UseParallelOldGC`

启用对完整的 gc 使用并行垃圾收集器。默认情况下，此选项是禁用的。自动启用它将启用 `-XX:+UseParallelGC` 选项。

- `-XX:+UseParNewGC`

允许在年轻一代中使用并行线程进行收集。默认情况下，此选项是禁用的。当您设置 `-XX:+UseConcMarkSweepGC` 选项时，它将自动启用。使用 `-XX:+UseParNewGC` 选项而不使用 `-XX:+UseConcMarkSweepGC` 选项在 JDK 8 中是不推荐的。

- `-XX:+UseSerialGC`

启用串行垃圾收集器。对于不需要从垃圾收集中获得任何特殊功能的小型和简单应用程序，这通常是最佳选择。默认情况下，这个选项是禁用的，收集器是根据机器的配置和 JVM 的类型自动选择的。

- `-XX:+UseStringDeduplication`

使字符串重复数据删除。默认情况下，此选项是禁用的。要使用此选项，必须启用 garbage-first (G1)垃圾收集器。请参见 `-XX:+UseG1GC` 选项。

通过利用许多字符串对象是相同的这一事实，字符串重复数据删除减少了 Java 堆上字符串对象的内存占用。不同于每个 String 对象指向自己的字符数组，相同的 String 对象可以指向并共享相同的字符数组。

- `-XX:+UseTLAB`

支持在年轻代空间中使用线程本地分配块(TLABs)。默认情况下启用此选项。要禁用 TLABs 的使用，请指定 `-XX:-UseTLAB`。

> 性能调优示例

下面的示例展示了如何使用实验性调优标志来优化吞吐量或提供更短的响应时间。

**例1 -调优以获得更高的吞吐量**

```bash
java -d64 -server -XX:+AggressiveOpts -XX:+UseLargePages -Xmn10g  -Xms26g -Xmx26g
```

**例2 -调优以降低响应时间**

```bash
java -d64 -XX:+UseG1GC -Xms26g Xmx26g -XX:MaxGCPauseMillis=500 -XX:+PrintGCTimeStamp
```

> Large Pages

也称为大页面，大页面是比标准内存页面大小(根据处理器和操作系统的不同而不同)大得多的内存页面。大页面优化处理器翻译后备缓冲区。

翻译后备缓冲区(TLB)是一个页面翻译缓存，它包含最近使用的虚拟到物理地址转换。TLB 是一种稀缺的系统资源。由于处理器必须从分层页表中读取数据，这可能需要多次内存访问，因此 TLB 错误可能会造成很高的代价。通过使用更大的内存页大小，单个 TLB 条目可以表示更大的内存范围。对 TLB 的压力将更小，内存密集型应用程序可能具有更好的性能。

然而，大页面页面内存会对系统性能产生负面影响。例如，当应用程序占用大量内存时，可能会造成常规内存不足，导致其他应用程序分页过多，并降低整个系统的速度。此外，一个运行了很长时间的系统可能会产生过多的碎片，这可能导致无法保留足够大的页面内存。当发生这种情况时，操作系统或 JVM 将恢复到使用常规页面。

> 应用程序类数据共享 CDS

这是一个商业特性，需要您还指定 `-XX:+UnlockCommercialFeatures` 选项。这也是一个实验特性;它可能会在未来的版本中发生变化。

## javac

> 读取 Java 类和接口定义，并将它们编译为字节码和类文件。

> 概要

```bash
# 参数可按任意次序排列
# options 命令行选项
# sourcefiles 一个或多个要编译的源文件（例如 Hello.java）
# @argfiles 一个或多个对源文件进行列表的文件
javac [options] [sourcefiles] [classes] [@argfiles]
```

> 描述

`javac` 命令读取用 Java 编程语言编写的类和接口定义，并将它们编译成字节码类文件。`javac` 命令还可以处理 Java 源文件和类中的注解。<br>
有两种方法可以将源代码文件名传递给 `javac`:

1. 如果源文件数量少，在命令行上列出文件名即可。
2. 如果源文件数量多，则将源文件列在一个文件中，名称间用空格或者回车进行分割。然后再 `javac` 命令行中使用该列表文件名，文件名前冠以 `@` 字符。

源代码文件名必须有 `.java` 后缀，类文件名必须有 `.class` 后缀，源文件和类文件都必须有标识类的根名称。例如，一个名为 *MyClass* 的类将在一个名为 *MyClass.java* 的源文件中编写，并编译成一个名为 *MyClass.class* 的字节码类文件。

内部类定义生成额外的类文件。这些类文件的名称结合了内部和外部类名，例如 *MyClass$MyInnerClass.class*。

将源文件排列在反映其包树的目录树中。例如，如果所有的源文件都在 */workspace* 中，那么将 *com.mysoft.mypack.MyClass* 的源代码放在 */workspace/com/mysoft/mypack/MyClass.java* 文件里面。

默认情况下，编译器将每个类文件放在与其源文件相同的目录中。您可以使用 `-d` 选项指定单独的目标目录。

> 选项

编译器有一组在当前开发环境中支持的标准选项。另外一组非标准选项是特定于当前虚拟机和编译器实现的，将来可能会发生更改。非标准选项以 `-X` 选项开始。

**标准选项**

- `-Akey[=value]`

指定传递给注解处理器的选项。这些选项不是由 `javac` 直接解释的，而是提供给各个处理器使用的。键值应该是由点(`.`)分隔的一个或多个标识符。

- `-cp path or -classpath path`

指定在何处查找用户类文件，以及(可选)注解处理器和源文件。这个选项将会覆盖用户系统的环境变量 *CLASSPATH*。如果没有指定 *CLASSPATH*、`-cp` 或 `-classpath`，则用户类路径是当前目录。

如果没有指定 `-sourcepath` 选项，那么还将搜索用户类路径以查找源文件。

如果没有指定 `-processorpath` 选项，则还将搜索类路径以查找注解处理器。

- `-Djava.ext.dirs=directories`

覆盖已安装扩展的位置。

- `-Djava.endorsed.dirs=directories`

覆盖已认可的标准路径的位置。

- `-d directory`

设置类文件的目标目录。该目录必须已经存在，因为 `javac` 不创建它。如果类是包的一部分，那么 `javac` 将类文件放在反映包名称的子目录中，并根据需要创建目录。

如果指定 `-d C:\myclasses`，且该类名为 `com.mypackage.MyClass`。则类文件是 `C:\myclasses\com\mypackage\MyClass.class`。

如果没有指定 `-d` 选项，那么 `javac` 将每个类文件放在与生成类文件的源文件相同的目录中。

注意: `-d` 选项指定的目录不会自动添加到用户类路径中。

- `-encoding encoding`

设置源文件编码名称，如 EUC-JP 和 UTF-8。如果未指定 `-encoding` 选项，则使用平台默认转换器。

- `-endorseddirs directories`

覆盖已认可的标准路径的位置。

- `-extdirs directories`

覆盖 ext 目录的位置。目录变量是一个用冒号分隔的目录列表。搜索指定目录中的每个 JAR 文件以查找类文件。所有找到的 JAR 文件都成为类路径的一部分。

- `-g`

生成所有调试信息，包括局部变量。默认情况下，只生成行号和源文件信息。

- `-g:none`

不生成任何调试信息。

- `-g:[keyword list]`

只生成由逗号分隔的关键字列表指定的某些调试信息。有效的关键词是:<br>
`source`: 源文件调试信息。<br>
`lines`: 行号调试信息。<br>
`vars`: 局部变量调试信息。<br>

- `-help`

打印标准选项的概要。

- `-implicit:[class, none]`

控制为隐式加载的源文件生成类文件。要自动生成类文件，请使用 `-implicit:class`。若要禁止类文件生成，请使用 `-implicit:none`。如果未指定此选项，则默认为自动生成类文件。在这种情况下，如果在进行注解处理时生成了此类文件，编译器将发出警告。当显式设置 `-implicit` 选项时，不会发出警告。

- `-Joption`

将选项传递给 Java 虚拟机(JVM)，其中的选项是 Java 启动程序参考页面中描述的选项之一。例如，`-J-Xms48m` 将启动内存设置为 48MB。

注意: *CLASSPATH*、`-classpath`、`-bootclasspath` 和 `-extdirs` 选项不指定用于运行 `javac` 的类。尝试使用这些选项和变量自定义编译器实现是有风险的，并且常常不能完成您想要的。如果必须自定义编译器实现，则使用 `-J` 选项将选项传递给底层Java启动程序。

- `-nowarn`

禁用警告消息。此选项的操作与 `-Xlint:none` 选项相同。

- `-parameters`

在生成的类文件中存储构造函数和方法的形式参数名，以便反射 API 中的`java.lang.reflect.Executable.getParameters` 方法可以检索它们。

- `-proc: [none, only]`

控制是否完成注解处理和编译。`-proc:none` 表示编译不需要注释处理。`-proc:only`  表示只进行注释处理，不进行任何后续编译。

- `-processor class1 [,class2,class3...]`

要运行的注解处理器的名称。这将绕过默认的发现过程。

- `-processorpath path`

指定在何处找到注解处理器。如果不使用此选项，则搜索类路径以查找处理器。

- `-s dir`

指定放置生成的源文件的目录。该目录必须已经存在，因为 `javac` 不创建它。如果类是包的一部分，那么编译器将源文件放在反映包名称的子目录中，并根据需要创建目录。

如果您指定 `-s C:\mysrc`，并且类名为 `com.mypackage.MyClass`。则将源文件放入 `C:\mysrc\com\mypackage\MyClass.java`。

- `-source release`

指定接受的源代码版本。允许释放以下值:<br>
`1.3`: 编译器不支持断言、泛型或 Java SE 1.3 之后引入的其他语言特性。<br>
`1.4`: 编译器接受包含断言的代码，这些断言是在 Java SE 1.4 中引入的。<br>
`1.5`: 编译器接受包含泛型和 Java SE 5 中引入的其他语言特性的代码。<br>
`5`: 同 `1.5`。<br>
`1.6`: Java SE 6 中没有引入任何语言更改。然而，源文件中的编码错误现在被报告为错误，而不是像在 Java Platform Standard Edition 的早期版本中那样被报告为警告。<br>
`6`: 同 `1.6`。<br>
`1.7`: 编译器接受带有 Java SE 7 中引入的特性的代码。<br>
`7`: 同 `1.7`。<br>
`1.8`: 编译器接受带有 Java SE 8 中引入的特性的代码。<br>
`8`: 同 `1.8`。<br>

- `-sourcepath sourcepath`

指定搜索类或接口定义的源代码路径。与用户类路径一样，在 Oracle Solaris 上，源路径条目用冒号(:)分隔，在 Windows 上用分号(;)分隔，可以是目录、JAR 归档文件或 ZIP 归档文件。如果使用包，则目录或归档文件中的本地路径名必须反映包名。

注意: 通过类路径找到的类可能会在找到它们的源文件时重新编译。

- `-verbose`

使用详细输出，其中包括有关加载的每个类和编译的每个源文件的信息。

- `-version`

打印版本信息。

- `-werror`

发生警告时终止编译。

- `-X`

显示关于非标准选项和出口的信息。

**交叉编译选项**

默认情况下，类是根据 `javac` 附带的平台的引导程序和扩展类编译的。但是 `javac` 也支持交叉编译，其中类是根据不同 Java 平台实现的引导程序和扩展类编译的。在交叉编译时，使用 `-bootclasspath` 和 `-extdirs` 选项非常重要。

- `-target version`

生成针对虚拟机指定版本的类文件。类文件将在指定的目标上和以后的版本上运行，但不会在 JVM 的早期版本上运行。有效的目标是 `1.1`、`1.2`、`1.3`、`1.4`、`1.5`(也是 `5`)、`1.6`(也是 `6`)、`1.7`(也是 `7`)和 `1.8`(也是 `8`)。

与 `-source` 相匹配。

- `-bootclasspath bootclasspath`

根据指定的引导类集交叉编译。与用户类路径一样，引导类路径条目由冒号(:)分隔，可以是目录、JAR 归档文件或 ZIP 归档文件。

**紧凑的配置文件选项**

从 JDK 8 开始，`javac` 编译器支持紧凑的概要文件。使用紧凑的概要文件，不需要整个 Java 平台的应用程序可以以更小的占用空间部署和运行。compact profiles 功能可以用来缩短应用程序商店的下载时间。该特性使捆绑 JRE 的 Java 应用程序的部署更加紧凑。这个特性在小型设备中也很有用。

所支持的概要文件值是 compact1、compact2 和 compact3。这些是附加层。每个编号较高的紧凑概要文件都包含具有较小编号名称的概要文件中的所有 APIs。

- `-profile`

当使用压缩概要文件时，此选项指定编译时的概要文件名称。例如:

```bash
javac -profile compact1 Hello.java
```

`javac` 不编译使用任何不在指定概要文件中的 Java SE APIs 的源代码。下面是一个错误消息的例子，导致试图编译这样的源代码:

```bash
cd jdk1.8.0/bin
./javac -profile compact1 Paint.java
Paint.java:5: error: Applet is not available in profile 'compact1'
import java.applet.Applet;
```

在本例中，可以通过修改源代码使其不使用 Applet 类来纠正错误。您还可以通过不使用 `-profile` 选项编译来纠正错误。然后编译将针对全套 Java SE APIs 运行。(紧凑的概要文件中没有一个包含 Applet 类。)

使用压缩概要文件编译的另一种方法是使用 `-bootclasspath` 选项来指定 *rt.jar* 文件的路径，该文件指定概要文件的映像。相反，使用 `-profile` 选项不需要在编译时在系统上显示概要文件映像。这在交叉编译时很有用。

**非标准选项**

- `-Xbootclasspath/p:path`

向引导类路径添加后缀。

- `-Xbootclasspath/a:path`

为引导类路径添加前缀。

- `-Xbootclasspath/:path`

重写引导类文件的位置。

- `-Xdoclint:[-]group [/access]`

启用或禁用特定的检查组，其中 `group` 是以下值之一: `accessibility`、`syntax`、`reference`、`html` 或 `missing`。有关这些检查组的更多信息，请参见 `javadoc` 命令的 `-Xdoclint` 选项。在 `javac` 命令中，默认情况下禁用 `-Xdoclint` 选项。

变量 `access` 指定 `-Xdoclint` 选项检查的类和成员的最小可见性级别。它可以有以下值之一(按最不可见到最不可见的顺序): `public`、`protected`、`package` 和 `private`。例如，下面的选项检查访问级别受保护或更高(包括 protected、package 和 public)的类和成员(带有所有组检查):

```
-Xdoclint:all/protected
```

以下选项支持所有访问级别的所有组检查，但它不会检查具有访问级别包或更高(包括 package 和public)的类和成员的 HTML 错误:

```
-Xdoclint:all,-html/package
```

- `-Xdoclint:none`

禁用所有组的检查。

- `-Xdoclint:all[/access]`

启用所有组的检查。

- `-Xlint`

启用所有建议的警告。在这个版本中，建议启用所有可用的警告。

- `-Xlint:all`

启用所有建议的警告。在这个版本中，建议启用所有可用的警告。

- `-Xlint:none`

禁用所有警告。

- `-Xlint:name`

禁用警告的名字。

- `-Xlint:-name`

禁用警告的名字。

- `-Xmaxerrs number`

设置要打印的最大错误数。

- `-Xmaxwarns number`

设置要打印的警告的最大数量。

- `-Xstdout filename`

向指定文件发送编译器消息。默认情况下，编译器消息转到 `System.err`。

- `-Xprefer:[newer,source]`

指定在为类型找到源文件和类文件时要读取哪个文件。如果使用 `-Xprefer:newer` 选项，那么它将读取源文件或类文件中较新的类型(默认)。如果使用 `-Xprefer:source` 选项，那么它将读取源文件。当您希望确保任何注解处理器都可以访问使用 *SOURCE* 保留策略声明的注释时，请使用 `-Xprefer:source`。

- `-Xpkginfo:[always,legacy,nonempty]`

控制 `javac` 是否从 *package-info.java* 文件生成 *package-info.class* 文件。此选项的可能模式参数包括以下内容。<br>
`always`: 始终为每个 *package-info.java* 文件生成一个 *package-info.class* 文件。如果使用 Ant 这样的构建系统，这个选项可能很有用，因为 Ant 检查每个 *.java* 文件是否有对应的 *.class* 文件。<br>
`legacy`: 仅当 *package-info.java* 包含注解时才生成 *package-info.class* 文件。如果 *package-info.java* 只包含注释，则不要生成 *package-info.class* 文件。注意: 可以生成一个 *package-info.class* 文件，但是如果 *package-info.java* 文件中的所有注解都有 `RetentionPolicy.SOURCE`，那么这个文件就是空的。<br>
`nonempty`: 只有当 *package-info.java* 包含带有 `RetentionPolicy.CLASS` 或 `RetentionPolicy.RUNTIME` 的注释时，才生成 *package-info.class* 文件。

- `-Xprint`

为调试目的打印指定类型的文本表示形式。既不执行注解处理，也不执行编译。输出的格式可能会改变。

- `-XprintProcessorInfo`

打印有关要求处理器处理哪些注解的信息。

- `-XprintRounds`

打印关于初始和后续注解处理器的信息。

> 使用 `-Xlint` 选项启用或禁用警告

使用 `-Xlint:name` 选项启用警告名称，其中 `name` 是以下警告名称之一。注意，您可以使用 `-Xlint:-name:` 选项禁用警告。

- `cast`

警告不必要和冗余的强制转换，例如:

```java
String s = (String) "Hello!"
```

- `classfile`

警告与类文件内容相关的问题。

- `deprecation`

警告使用废弃的方法或类，例如:

```java
java.util.Date myDate = new java.util.Date();
int currentDay = myDate.getDay();
```

`java.util.Date.getDay` 方法从 JDK 1.1 开始就被弃用了

- `dep-ann`

警告使用 `@deprecated` Javadoc 注释记录但没有 `@Deprecated` 注解的项，例如:

```java
/**
  * @deprecated As of Java SE 7, replaced by {@link #newMethod()}
  */
public static void deprecatedMethood() { }
public static void newMethod() { }
```

- `divzero`

警告除以 0，例如:

```java
int divideByZero = 42 / 0;
```

- `empty`

警告 `if` 语句之后的空语句，例如:

```java
class E {
    void m() {
         if (true) ;
    }
}
```

- `fallthrough`

检查切换块是否存在故障，并为找到的任何故障提供警告消息。切换用例是切换块中的用例，而不是块中的最后一个用例，它的代码不包含 break 语句，允许代码执行从该用例切换到下一个用例。例如，这个开关块中 case 1 标签后面的代码并没有以 break 语句结束:

```java
switch (x) {
case 1:
  System.out.println("1");
  // No break statement here.
case 2:
  System.out.println("2");
}
```

如果在编译这段代码时使用了 `-Xlint:fallthrough` 选项，那么编译器会发出一个警告，提示可能的 case 跳转，并给出有关 case 的行号。

- `finally`

警告不能正常完成 *finally* 子句，例如:

```java
public static int m() {
    try {
       throw new NullPointerException();
    } catch (NullPointerException(); {
        System.err.println("Caught NullPointerException.");
        return 1;
    } finally {
        return 0;
    }
}
```

编译器为本例中的 *finally* 块生成一个警告。当调用 int 方法时，它返回一个值 0。当 try 块退出时，将执行 *finally* 块。在本例中，当控件被转移到 *catch* 块时，int 方法退出。然而，*finally* 块必须执行，因此它被执行，即使控制被转移到方法之外。

- `options`

警告与使用命令行选项相关的问题。看到交叉编译选项。

- `overrides`

警告有关方法重写的问题。例如，考虑以下两个类:

```java
public class ClassWithVarargsMethod {
  void varargsMethod(String... s) { }
}

public class ClassWithOverridingMethod extends ClassWithVarargsMethod {
   @Override
   void varargsMethod(String[] s) { }
}
```

编译器生成一个类似于下面的警告:

```
warning: [override] varargsMethod(String[]) in ClassWithOverridingMethod 
overrides varargsMethod(String...) in ClassWithVarargsMethod; overriding
method is missing '...'
```

当编译器遇到 `ClassWithVarargsMethod.varargsMethod(String...)` 方法时，它将 *varargsMethod* 方法的形式参数 `String...` 转换为 `String[]`。而当编译器遇到重载方法 `ClassWithOverridingMethod.varargsMethod(String[])` 时也会尝试上面动作。

- `path`

警告命令行上的无效路径元素和不存在的路径目录(关于类路径、源路径和其他路径)。这些警告不能用 `@SuppressWarnings` 注解加以抑制，例如:

```cmd
javac -Xlint:path -classpath C:\nonexistentpath Example.java
```

- `processing`

警告有关注解处理的问题。当您有一个具有注解的类，并且您使用的注解处理器不能处理这种类型的异常时，编译器将生成此警告。例如，下面是一个简单的注解处理器:

```java
/**
 * Source file AnnocProc.java
 */
import java.util.*;
import javax.annotation.processing.*;
import javax.lang.model.*;
import java.lang.model.element.*;

@SupportedAnnotationTypes("NotAnno")
public class AnnoProc extends AbstractProcessor {
  public boolean process(Set<? extends TypeElement> elems, RoundEnvironment renv){
     return true;
  }

  public SourceVersion getSupportedSourceVersion() {
     return SourceVersion.latest();
   }
}

/**
 * Source file AnnosWithoutProcessors.java
 */
@interface Anno { }
 
@Anno
class AnnosWithoutProcessors { }
```

下面的命令编译注释处理器 *AnnoProc*，然后对源文件 *Annoswithoutprocessor.java* 运行这个注释处理器:

```cmd
javac AnnoProc.java
javac -cp . -Xlint:processing -processor AnnoProc -proc:only AnnosWithoutProcessors.java
```

当编译器对源文件 *AnnosWithoutProcessors.java* 运行注解处理器时，它会产生以下警告:

```cmd
warning: [processing] No processor claimed any of these annotations: Anno
```

要解决这个问题，可以将在 *AnnosWithoutProcessors* 类中定义和使用的注解从 *Anno* 重命名为 *NotAnno*。

- `rawtypes`

警告未选中的原始类型操作。下面的语句生成一个 *rawtypes* 警告:

```java
void countElements(List l) { ... }
```

下面的示例不会生成 *rawtypes* 警告

```java
void countElements(List<?> l) { ... }
```

*List* 是一个原始类型。然而, *List<?>* 是一个无界通配符参数化类型。因为 *List* 是一个参数化的接口，所以总是指定它的类型参数。在本例中，*List* 形式参数指定为无界通配符(`?`)作为其形式类型参数，这意味着 *countElements* 方法可以接受列表接口的任何实例化。

- `Serial`

警告可序列化类上缺少 *serialVersionUID* 定义，例如:

```java
public class PersistentTime implements Serializable {
  private Date time;
 
   public PersistentTime() {
     time = Calendar.getInstance().getTime();
   }
 
   public Date getTime() {
     return time;
   }
}
```

编译器生成以下警告:

```cmd
warning: [serial] serializable class PersistentTime has no definition of
serialVersionUID
```

如果可序列化类没有显式声明名为 *serialVersionUID* 的字段，则序列化运行时环境根据类的各个方面计算该类的默认 *serialVersionUID* 值，如 Java 对象序列化规范中所述。但是，强烈建议所有可序列化的类都显式声明 *serialVersionUID* 值，因为计算 *serialVersionUID* 值的默认过程对类细节非常敏感，类细节可能因编译器实现的不同而不同，因此可能会导致意外的失效

- `static`

警告有关使用 static 方法的问题，例如:

```java
class XLintStatic {
    static void m1() { }
    void m2() { this.m1(); }
}
```

编译器生成以下警告:

```cmd
warning: [static] static method should be qualified by type name, 
XLintStatic, instead of by an expression
```

为了解决这个问题，你可以调用静态方法 *m1* 如下:

```java
XLintStatic.m1();
```

- `try`

警告有关使用 *try* 块的问题，包括 *try-with-resources* 语句。例如，由于没有使用 *try* 块中声明的资源 *ac*，因此会为下面的语句生成一个警告:

```java
try (AutoCloseable ac = getResource()) {}
```

- `unchecked`

提供 Java 语言规范规定的未检查转换警告的更多细节，例如:

```java
List l = new ArrayList<Number>();
List<String> ls = l;  // unchecked warning
```

在类型擦除期间，`ArrayList<Number>` 和 `List<String>` 类型分别变为 `ArrayList` 和 `List`。

- `varargs`

警告变量参数(varargs)方法的不安全使用，特别是那些包含不可具体化参数的方法，例如:

```java
public class ArrayBuilder {
  public static <T> void addToList (List<T> listArg, T... elements) {
    for (T x : elements) {
      listArg.add(x);
    }
  }
}
```

注意: 不可具体化类型是在运行时类型信息不完全可用的类型。

> 命令行参数文件

要缩短或简化 `javac` 命令，可以指定一个或多个文件，其中包含 `javac` 命令的参数(`-J` 选项除外)。这使您能够在任何操作系统上创建任意长度的 `javac` 命令。

参数文件可以包含任何组合中的 `javac` 选项和源文件名。文件中的参数可以用空格或新行字符分隔。如果文件名包含嵌入式空格，则将整个文件名放在双引号中。

参数文件中的文件名相对于当前目录，而不是参数文件的位置。这些列表中不允许使用通配符(`*`)(例如用于指定 **.java*)。不支持使用 at 符号(`@`)递归地解释文件。不支持 `-J` 选项，因为它们被传递给启动程序，启动程序不支持参数文件。

在执行 `javac` 命令时，传入每个参数文件的路径和名称，并以 at 符号(`@`)为前导字符。当 `javac` 命令遇到以 at 符号(`@`)开头的参数时，它将该文件的内容展开到参数列表中。

**例1 -单参数文件**

您可以使用一个名为 *argfile* 的参数文件来保存所有 `javac` 参数:

```cmd
javac @argfile
```

这个参数文件可以包含示例 2 中所示的两个文件的内容

**例2 -两个参数文件**

您可以创建两个参数文件: 一个用于 `javac` 选项，另一个用于源文件名。注意，以下列表没有行延续字符。

创建一个名为 options 的文件，该文件包含以下内容:

```
-d classes
-g
-sourcepath C:\java\pubs\ws\1.3\src\share\classes
```

创建一个名为 classes 的文件，其中包含以下内容:

```
MyClass1.java
MyClass2.java
MyClass3.java
```

然后，运行 `javac` 命令如下:

```cmd
javac @options @classes
```

**例3 -带路径的参数文件**

参数文件可以有路径，但是文件中的任何文件名都是相对于当前工作目录的(不是 path1 或 path2):

```cmd
javac @path1/options @path2/classes
```

## javadoc

> 从 Java 源文件生成 API 文档的 HTML 页面。

> 概要

```bash
# packages 要记录的包的名称，用空格分隔，例如java.lang java.lang.reflect java.awt。如果还想记录子包，请使用 -subpackages 选项指定包。
#          默认情况下，javadoc 在当前目录和子目录中查找指定的包。使用 -sourcepath 选项指定查找包的目录列表。
# source-files 要记录的 Java 源文件的名称，用空格分隔，例如 Class.java Object.java Button.java。默认情况下，javadoc 在当前目录中查找指定的类。但是，您可以指定类文件的完整路径并使用通配符，例如 /home/src/java/awt/Graphics*.java。还可以指定相对于当前目录的路径。
# options 命令行选项，用空格分隔。
# @argfiles 包含 javadoc 命令选项列表的文件的名称，包名和源文件名，按任意顺序排列。
javadoc {packages|source-files} [options] [@argfiles]
```

> 源文件

`javadoc` 命令生成的输出源自以下类型的源文件: 用于类的 Java 语言源文件(*.java*)、包注释文件、概述注释文件和其他未处理的文件。

**package-info.java**

```java
/**
 * Provides the classes necessary to create an  
 * applet and the classes an applet uses 
 * to communicate with its applet context.
 * <p>
 * The applet framework involves two entities:
 * the applet and the applet context.
 * An applet is an embeddable window (see the
 * {@link java.awt.Panel} class) with a few extra
 * methods that the applet context can use to 
 * initialize, start, and stop the applet.
 *
 * @since 1.0
 * @see java.awt
 */
package java.lang.applet;
```

**package.html**

```html
<HTML>
<BODY>
Provides the classes necessary to create an applet and the 
classes an applet uses to communicate with its applet context.
<p>
The applet framework involves two entities: the applet
and the applet context. An applet is an embeddable
window (see the {@link java.awt.Panel} class) with a
few extra methods that the applet context can use to
initialize, start, and stop the applet. 

@since 1.0 
@see java.awt
</BODY>
</HTML>
```

## ~~javah~~

> @delete 11<br>
> @see [javac -h](#javac)<br>
> 从 Java 类生成 C 头文件和源文件。

> 概要

```bash
# options 命令行选项。
# fully-qualified-class-name 要转换为 C 头文件和源文件的类的完全限定位置。
javah [options] fully-qualified-class-name ...
```

> 描述

`javah` 命令生成实现本机方法所需的 C 头文件和源文件。C 程序使用生成的头文件和源文件从本机源代码引用对象的实例变量。*.h* 文件包含一个 *struct* 定义，其布局与对应类的布局平行。*struct* 中的字段对应于类中的实例变量。

头文件的名称及其内部声明的结构派生自类的名称。当传递给 `javah` 命令的类位于包中时，包名被添加到头文件名和结构名称的开头。下划线(`_`)用作名称分隔符。

默认情况下，`javah` 命令为命令行中列出的每个类创建一个头文件，并将文件放在当前目录中。使用 `-stubs` 选项创建源文件。使用 `-o` 选项将列出的所有类的结果连接到一个文件中。

Java 本机接口(JNI)不需要头信息或存根文件。`javah` 命令仍然可以用来生成 JNI-style 的本机方法所需的本机方法函数原型。`javah` 命令默认情况下生成 JNI-style 的输出，并将结果放在 *.h* 文件中。

## javap

> 反汇编一个或多个类文件。

> 概要

```bash
# options 命令行选项。
# classfile 用空格分隔的一个或多个类，用于处理 DocFooter.class 等注解。您可以指定一个可以在类路径中找到的类，通过它的文件名或 URL，例如 file:///home/user/myproject/src/DocFooter.class。
javap [options] classfile...
```

> 描述

`javap` 命令反汇编一个或多个类文件。输出取决于所使用的选项。当不使用任何选项时，`javap` 命令将打印传递给它的包、受保护的和公共字段，以及类的方法。`javap` 命令将输出打印到 *stdout*。

> 选项

- `-help`
- `--help`
- `?`

为 `javap` 命令打印一条帮助消息。

- `-version`

打印版本信息。

- `-l`

打印行和局部变量表。

- `-public`

只显示公共类和成员。

- `-protected`

只显示受保护的类和公共类以及成员。

- `-private`
- `-p`

显示所有类和成员。

- `-Joption`

将指定的选项传递给 JVM。例如:

```bash
javap -J-version
javap -J-Djava.security.manager -J-Djava.security.policy=MyPolicy MyClassName
```

有关 JVM 选项的更多信息，请参见 `java` 命令文档。

- `-s`

打印内部类型签名。

- `-sysinfo`

显示正在处理的类的系统信息(路径、大小、日期、MD5 散列)。

- `-constants`

显示 *static final* 常数。

- `-c`

为类中的每个方法打印分解后的代码，例如，包含 Java 字节码的指令。

- `-verbose`

打印堆栈大小、局部变量数量和方法参数。

- `-bootclasspath path`

指定加载引导类的路径。默认情况下，引导类是实现位于 jre/lib/rt.jar 和其他几个 JAR 文件中的核心 Java 平台的类。

- `-extdir dirs`

覆盖搜索已安装扩展的位置。扩展的默认位置是 java.ext.dirs 的值。

> 例子

编译以下 DocFooter 类:

```java
import java.awt.*;
import java.applet.*;
 
public class DocFooter extends Applet {
        String date;
        String email;
 
        public void init() {
                resize(500,100);
                date = getParameter("LAST_UPDATED");
                email = getParameter("EMAIL");
        }
 
        public void paint(Graphics g) {
                g.drawString(date + " by ",100, 15);
                g.drawString(email,290,15);
        }
}
```

使用 `javap DocFooter.class` 命令将输出如下：

```bash
Compiled from "DocFooter.java"
public class DocFooter extends java.applet.Applet {
  java.lang.String date;
  java.lang.String email;
  public DocFooter();
  public void init();
  public void paint(java.awt.Graphics);
}
```

`javap -c DocFooter.class`

```bash
Compiled from "DocFooter.java"
public class DocFooter extends java.applet.Applet {
  java.lang.String date;
  java.lang.String email;

  public DocFooter();
    Code:
       0: aload_0       
       1: invokespecial #1                  // Method
java/applet/Applet."<init>":()V
       4: return        

  public void init();
    Code:
       0: aload_0       
       1: sipush        500
       4: bipush        100
       6: invokevirtual #2                  // Method resize:(II)V
       9: aload_0       
      10: aload_0       
      11: ldc           #3                  // String LAST_UPDATED
      13: invokevirtual #4                  // Method
 getParameter:(Ljava/lang/String;)Ljava/lang/String;
      16: putfield      #5                  // Field date:Ljava/lang/String;
      19: aload_0       
      20: aload_0       
      21: ldc           #6                  // String EMAIL
      23: invokevirtual #4                  // Method
 getParameter:(Ljava/lang/String;)Ljava/lang/String;
      26: putfield      #7                  // Field email:Ljava/lang/String;
      29: return        

  public void paint(java.awt.Graphics);
    Code:
       0: aload_1       
       1: new           #8                  // class java/lang/StringBuilder
       4: dup           
       5: invokespecial #9                  // Method
 java/lang/StringBuilder."<init>":()V
       8: aload_0       
       9: getfield      #5                  // Field date:Ljava/lang/String;
      12: invokevirtual #10                 // Method
 java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      15: ldc           #11                 // String  by 
      17: invokevirtual #10                 // Method
 java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: invokevirtual #12                 // Method
 java/lang/StringBuilder.toString:()Ljava/lang/String;
      23: bipush        100
      25: bipush        15
      27: invokevirtual #13                 // Method
 java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      30: aload_1       
      31: aload_0       
      32: getfield      #7                  // Field email:Ljava/lang/String;
      35: sipush        290
      38: bipush        15
      40: invokevirtual #13                 // Method
java/awt/Graphics.drawString:(Ljava/lang/String;II)V
      43: return        
}
```

## jdeprscan

> @since 9<br>
> 您可以使用 `jdeprscan` 工具作为静态分析工具来扫描 jar 文件（或其他类文件聚合）以使用已弃用的 API 元素。

## jdeps

> Java 类依赖性分析器。

> 概要

```bash
# options 选项
# classes 要分析的类的名称。您可以通过类的文件名、目录或 JAR 文件来指定可以在类路径中找到的类。
jdeps [options] classes ...
```

> 描述

`jdeps` 命令显示 Java 类文件的*包级*或*类级*依赖关系。输入类可以是 *.class* 文件、目录、JAR 文件的路径名，也可以是分析所有类文件的完全限定类名。选项决定输出。默认情况下，`jdeps` 将依赖项输出到系统输出。它可以在 DOT 语言中生成依赖项(参见 `-dotoutput` 选项)。

> 选项

- `-dotoutput <dir>`

点文件输出的目标目录。如果指定，`jdeps` 将为每个分析的归档文件生成一个点文件，名为 `<archive-file-name>`。点号列出依赖项，以及一个名为 summary 的摘要文件。点列出归档之间的依赖关系。

- `-s`
- `-summary`

仅打印依赖项摘要。

- `-v`
- `-verbose`

打印所有类级依赖项。

- `-verbose:package`

打印包级别的依赖项(不包括同一归档中的依赖项)。

- `-verbose:class`

打印类级别的依赖项(不包括同一归档中的依赖项)。

- `-p <pkg name>`
- `-package <pkg name>`

查找指定包中的依赖项。您可以为不同的包多次指定此选项。`-p` 和 `-e` 选项是互斥的。

- `-e <regex>`
- `-regex <regex>`

查找与指定正则表达式模式匹配的包中的依赖项。`-p` 和 `-e` 选项是互斥的。

- `-include <regex>`

将分析限制为类匹配模式。此选项筛选要分析的类列表。它可以与 `-p` 和 `-e` 一起使用，它们将模式应用于依赖项。

- `-jdkinternals`

在 JDK 内部 api 中找到类级依赖项。默认情况下，它会分析 `-classpath` 选项和输入文件中指定的所有类，除非您指定了 `-include` 选项。不能将此选项与 `-p`、`-e` 和 `-s` 选项一起使用。

警告: JDK 内部 api 可能在即将发布的版本中不可访问。

- `-P`
- `-profile`

显示配置文件或包含包的文件。

- `-apionly`

将分析限制到 api，例如，依赖于公共类的公共成员和受保护成员的签名，包括字段类型、方法参数类型、返回类型和检查异常类型。

- `-R`
- `-recursive`

递归遍历所有依赖项。

> 例子

分析 Notepad.jar 的依赖关系。

```bash
jdeps demo\jfc\Notepad\Notepad.jar
```

```bash
demo\jfc\Notepad\Notepad.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   <unnamed> (Notepad.jar)
      -> java.awt                                           
      -> java.awt.event                                     
      -> java.beans                                         
      -> java.io                                            
      -> java.lang                                          
      -> java.net                                           
      -> java.util                                          
      -> java.util.logging                                  
      -> javax.swing                                        
      -> javax.swing.border                                 
      -> javax.swing.event                                  
      -> javax.swing.text                                   
      -> javax.swing.tree                                   
      -> javax.swing.undo 
```

使用 `-P` 或 `-profile` 选项来显示记事本所依赖的配置文件。

```bash
$ jdeps -profile demo\jfc\Notepad\Notepad.jar
demo\jfc\Notepad\Notepad.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar (Full JRE)
   <unnamed> (Notepad.jar)
      -> java.awt                                           Full JRE
      -> java.awt.event                                     Full JRE
      -> java.beans                                         Full JRE
      -> java.io                                            compact1
      -> java.lang                                          compact1
      -> java.net                                           compact1
      -> java.util                                          compact1
      -> java.util.logging                                  compact1
      -> javax.swing                                        Full JRE
      -> javax.swing.border                                 Full JRE
      -> javax.swing.event                                  Full JRE
      -> javax.swing.text                                   Full JRE
      -> javax.swing.tree                                   Full JRE
      -> javax.swing.undo                                   Full JRE
```

分析给定类路径中特定类的直接依赖关系，例如 *com.sun.tools.jdeps.Main* jar文件中的 Main 类。

```bash
$ jdeps -cp lib\tools.jar com.sun.tools.jdeps.Main
lib\tools.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   com.sun.tools.jdeps (tools.jar)
      -> java.io                                            
      -> java.lang 
```

使用 `-verbose:class` 选项来查找类级依赖项，或者使用 `-v` 或 `-verbose` 选项来包含来自同一个 JAR 文件的依赖项。

```bash
$ jdeps -verbose:class -cp lib\tools.jar com.sun.tools.jdeps.Main
 
lib\tools.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   com.sun.tools.jdeps.Main (tools.jar)
      -> java.io.PrintWriter                                
      -> java.lang.Exception                                
      -> java.lang.Object                                   
      -> java.lang.String                                   
      -> java.lang.System 
```

使用 `-R` 或 `-recursive` 选项来分析 *com.sun.tools.jdeps.Main* 的传递依赖关系。主类。

```bash
$ jdeps -R -cp lib\tools.jar com.sun.tools.jdeps.Main
lib\tools.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   com.sun.tools.classfile (tools.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.reflect                                  
      -> java.nio.charset                                   
      -> java.nio.file                                      
      -> java.util                                          
      -> java.util.regex                                    
   com.sun.tools.jdeps (tools.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.nio.file                                      
      -> java.nio.file.attribute                            
      -> java.text                                          
      -> java.util                                          
      -> java.util.jar                                      
      -> java.util.regex                                    
      -> java.util.zip                                      
c:\Program Files\Java\jdk1.8.0\jre\lib\jce.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar
   javax.crypto (jce.jar)
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.reflect                                  
      -> java.net                                           
      -> java.nio                                           
      -> java.security                                      
      -> java.security.cert                                 
      -> java.security.spec                                 
      -> java.util                                          
      -> java.util.concurrent                               
      -> java.util.jar                                      
      -> java.util.regex                                    
      -> java.util.zip                                      
      -> javax.security.auth                                
      -> sun.security.jca                                   JDK internal API (rt.jar)
      -> sun.security.util                                  JDK internal API (rt.jar)
   javax.crypto.spec (jce.jar)
      -> java.lang                                          
      -> java.security.spec                                 
      -> java.util                                          
c:\Program Files\Java\jdk1.8.0\jre\lib\rt.jar -> c:\Program Files\Java\jdk1.8.0\jre\lib\jce.jar
   java.security (rt.jar)
      -> javax.crypto
```

生成点文件的依赖关系的记事本演示。

```bash
$ jdeps -dotoutput dot demo\jfc\Notepad\Notepad.jar
```

`jdeps` 将为每个名为 <filename> 的给定 JAR 文件创建一个点文件。点在 `-dotoutput` 选项中指定的点目录中，还有一个名为 summary 的摘要文件，它将列出 JAR 文件之间的依赖关系

```bash
$ cat dot\Notepad.jar.dot 
digraph "Notepad.jar" {
    // Path: demo\jfc\Notepad\Notepad.jar
   "<unnamed>"                                        -> "java.awt";
   "<unnamed>"                                        -> "java.awt.event";
   "<unnamed>"                                        -> "java.beans";
   "<unnamed>"                                        -> "java.io";
   "<unnamed>"                                        -> "java.lang";
   "<unnamed>"                                        -> "java.net";
   "<unnamed>"                                        -> "java.util";
   "<unnamed>"                                        -> "java.util.logging";
   "<unnamed>"                                        -> "javax.swing";
   "<unnamed>"                                        -> "javax.swing.border";
   "<unnamed>"                                        -> "javax.swing.event";
   "<unnamed>"                                        -> "javax.swing.text";
   "<unnamed>"                                        -> "javax.swing.tree";
   "<unnamed>"                                        -> "javax.swing.undo";
}
 
$ cat dot\summary.dot
digraph "summary" {
   "Notepad.jar"                  -> "rt.jar";
}
```

## jlink

> @since 9<br>
> 您可以使用 `jlink` 工具将一组模块及其依赖项组合和优化到自定义运行时映像中。

> 概要

```bash
# options 用空格分隔的命令行选项
# modulepath jlink 工具发现可观察模块的路径。这些模块可以是模块化 JAR 文件、JMOD 文件或分解模块。
# module 要添加到运行时映像的模块的名称。jlink 工具添加了这些模块及其传递依赖项。
jlink [options] --module-path modulepath --add-modules module [,module...]
```

> 描述

`jlink` 工具链接一组模块及其传递依赖项，以创建自定义运行时映像。

注意： 开发人员负责更新他们的自定义运行时映像。与自定义运行时映像不同，web 部署的 Java 应用程序在可用时自动从 web 下载应用程序更新。Java 自动更新机制负责每年多次将 JRE 更新到最新的安全版本。自定义运行时映像不支持自动更新。

> 选项

- `--add-modules mod [,mod...]`

将命名模块 mod 添加到默认的根模块集。默认的根模块集是空的。

- `--bind-services`

链接服务提供程序模块及其依赖项。

- `-c ={0|1|2}`
- `--compress={0|1|2}`

启用压缩资源:

*0*: 不压缩

*1*: 常量字符串分享

*2*: ZIP

- `--disable-plugin pluginname`

禁用指定的插件。

- `--endian {little|big}`

指定生成图像的字节顺序。默认值是系统体系结构的格式。

- `-h`
- `--help`

打印帮助信息

- `--ignore-signing-information`

在运行时映像中链接带符号的模块 jar 时，抑制致命错误。已签名模块 jar 的签名相关文件不会复制到运行时映像中。

- `--launcher command=module`
- `--launcher command=module/main`

指定模块的启动器命令名或模块和主类的命令名(模块和主类名之间用斜杠(/)分隔)。

- `--limit-modules mod [,mod...]`

将可观察模块的范围限制在命名模块 *mod* 的传递闭包中，如果有主模块，则加上 `——add-modules` 选项中指定的任何其他模块。

- `--list-plugins`

列出可用的插件，您可以通过命令行选项访问这些插件;

- `-p`
- `--module-path modulepath`

指定模块路径。

- `--no-header-files`

不包括头文件。

- `--no-man-pages`

不包括手册页。

- `--output path`

指定生成的运行时映像的位置。

- `--save-opts filename`

将 `jlink` 选项保存到指定的文件中。

- `--suggest-providers [name, ...]`

建议提供程序从模块路径实现给定的服务类型。

- `--version`

打印版本信息。

- `@filename`

从指定文件中读取选项。

选项文件是一个文本文件，包含您通常会在命令提示符中输入的选项和值。选项可能出现在一行，也可能出现在几行。您不能为路径名指定环境变量。您可以通过在行首加上一个散列符号(#)来注释行。

下面是 `jlink` 命令的选项文件示例:

```
#Wed Dec 07 00:40:19 EST 2016
--module-path C:/Java/jdk9/jmods;mlib
--add-modules com.greetings
--output greetingsapp
```

> 插件

注意：本节中未列出的插件不受支持，可能会发生更改。

对于需要模式列表的插件选项，值是由逗号分隔的元素列表，每个元素使用以下形式:

*glob-pattern*<br>
*glob:glob-pattern*<br>
*regex:regex-pattern*<br>
*@filename*<br>

要获得所有可用插件的完整列表，请运行该命令:

```
jlink --list-plugins
```

插件名|选项|描述
:---|:---|:---
class-for-name|`--class-for-name`|Class 优化，转换 `Class.forName` 调用常量负载。
compress|`--compress={0|1|2}[:filter=pattern-list]`|压缩输出映像中的所有资源。
dedup-legal-notices|`--dedup-legal-notices=[error-if-not-same-content]`|删除所有法律公告的副本。如果 `error-if-not-same-content` 指定，则如果相同文件名的两个文件不同，则为错误。
exclude-files|`--exclude-files=pattern-list`|指定要排除的文件。
exclude-jmod-section|`--exclude-jmod-section=section-name`|指定一个 JMOD 节，以排除节名为 man 或 header 的地方。
exclude-resources|`--exclude-resources=pattern-list`|指定要排除的资源。
generate-jli-classes|`--generate-jli-classes=@filename[:ignore-version=<true|false>]`|指定文件清单 `java.lang.invoke` 提前生成类。默认情况下，这个插件可以使用一个内置的类列表来预生成。如果这个插件运行在与创建的映像不同的运行时版本上，那么默认情况下代码生成将被禁用，以确保正确性。添加 `ignore-version=true` 以覆盖此行为。
include-locales|`--include-locales=langtag[,langtag]*`|包含 langtag 是 BCP 47 语言标记的语言环境列表。
order-resources|`--order-resources=pattern-list`|按优先级顺序排列指定的路径。如果指定了 @filename，那么模式列表中的每一行必须与要排序的路径完全匹配。
release-info|`--release-info={file|add:key1=value1:key2=value2:...|del:key-list}`|加载、添加或删除发布属性
strip-debug|`--strip-debug`|从输出图像中删除调试信息
strip-native-commands|`--strip-native-commands`|排除本机命令(例如 `java/java.exe` 
system-modules|`--system-modules=retainModuleTarget`|快速加载模块描述符(始终启用)
vm|`--vm={client|server|minimal|all}`|在输出映像中选择HotSpot VM。默认 all。

> 例子

下面的命令在 `greetingsapp` 目录中创建一个运行时映像。这个命令链接模块 *com.greetings*，其模块定义包含在目录 `mlib` 中。目录 `$JAVA_HOME/jmods` 包含 *java.base.jmod* 和其他标准和 JDK 模块。

```
jlink --module-path $JAVA_HOME/jmods:mlib --add-modules com.greetings --output greetingsapp
```

下面的命令列出了运行时映像 `greetingsapp` 中的模块:

```
greetingsapp/bin/java --list-modules
com.greetings
java.base@9
java.logging@9
org.astro@1.0
```

下面的命令在 `compressedrt` 目录中创建一个运行时映像，该目录去掉了调试符号，使用压缩来减少空间，并包含法语语言环境信息:

```
jlink --module-path $JAVA_HOME/jmods --add-modules jdk.localedata --strip-debug --compress=2 --include-locales=fr --output compressedrt
```

下面的例子比较了运行时图像 *compressedrt* 和 *fr_rt* 的大小， *fr_rt* 没有去掉调试符号，也不使用压缩:

```
jlink --module-path $JAVA_HOME/jmods --add-modules jdk.localedata --include-locales=fr --output fr_rt

du -sh ./compressedrt ./fr_rt
23M     ./compressedrt
36M     ./fr_rt
```

下面的例子列出了实现 *java.security.Provider* 的提供者:

```
jlink --module-path $JAVA-HOME/jmods --suggest-providers java.security.Provider

Suggested providers:
   java.naming provides java.security.Provider used by java.base
   java.security.jgss provides java.security.Provider used by java.base
   java.security.sasl provides java.security.Provider used by java.base
   java.smartcardio provides java.security.Provider used by java.base
   java.xml.crypto provides java.security.Provider used by java.base
   jdk.crypto.cryptoki provides java.security.Provider used by java.base
   jdk.crypto.ec provides java.security.Provider used by java.base
   jdk.crypto.mscapi provides java.security.Provider used by java.base
   jdk.deploy provides java.security.Provider used by java.base
   jdk.security.jgss provides java.security.Provider used by java.base
```

下面的示例创建一个名为 *mybuild* 的自定义运行时映像，该映像只包含 *java.naming* 和 *jdk.crypto.cryptoki* 及其依赖项，但没有其他提供程序。注意，这些依赖项必须存在于模块路径中:

```
jlink --module-path $JAVA_HOME/jmods --add-modules java.naming,jdk.crypto.cryptoki --output mybuild
```

下面的命令与创建一个名为 *greetingsapp* 的运行时映像的命令类似，只是它将用服务绑定链接从根模块解析的模块; 看 *Configuration.resolveAndBind* 方法。

```
jlink --module-path $JAVA_HOME/jmods:mlib --add-modules com.greetings --output greetingsapp --bind-services
```

下面的命令列出了这个命令创建的运行时映像 `greetingsapp` 中的模块:

```
greetingsapp/bin/java --list-modules
com.greetings
java.base@9
java.compiler@9
java.datatransfer@9
java.desktop@9
java.logging@9
java.management@9
java.management.rmi@9
java.naming@9
java.prefs@9
java.rmi@9
java.scripting@9
java.security.jgss@9
java.security.sasl@9
java.smartcardio@9
java.xml@9
java.xml.crypto@9
jdk.accessibility@9
jdk.charsets@9
jdk.compiler@9
jdk.crypto.cryptoki@9
jdk.crypto.ec@9
jdk.crypto.mscapi@9
jdk.deploy@9
jdk.dynalink@9
jdk.internal.opt@9
jdk.jartool@9
jdk.javadoc@9
jdk.jdeps@9
jdk.jlink@9
jdk.localedata@9
jdk.management@9
jdk.naming.dns@9
jdk.naming.rmi@9
jdk.scripting.nashorn@9
jdk.security.auth@9
jdk.security.jgss@9
jdk.unsupported@9
jdk.zipfs@9
org.astro@1.0
```

## jmod

> @since 9<br>
> 您可以使用 `jmod` 工具创建 JMOD 文件并列出现有 JMOD 文件的内容。

# 监控 Java 应用

## jconsole

> 启动一个图形控制台，可以监视和管理 Java 应用程序。

> 概要

```bash
# options 命令行选项。
# connection = pid | host:port | jmxURL 
#              pid值是本地 Java 虚拟机(JVM)的进程 ID。JVM 必须使用与运行 jconsole 命令的用户 ID 相同的用户 ID 运行。
#              host:port 值是运行 JVM 的主机系统的名称，以及 system 属性 com.sun.management.jmxremote.port 指定的端口号。JVM 启动时的端口。
#              jmxUrl 值是要连接到的 JMX 代理的地址，如 JMXServiceURL 中所述。
jconsole [options] [connection...]
```

> 描述

`jconsole` 命令启动一个图形化控制台工具，该工具允许您监视和管理本地或远程机器上的 Java 应用程序和虚拟机。

在 Windows 上，`jconsole` 命令不与控制台窗口关联。但是，当 `jconsole` 命令失败时，它会显示一个包含错误信息的对话框。

> 选项

- `-interval=n`

将更新间隔设置为 n 秒(默认为 4 秒)。

- `-notile`

最初不平铺窗口(用于两个或多个连接)。

- `-pluginpath plugins`

指定要搜索 *JConsole* 插件的目录或 JAR 文件列表。插件路径应该包含一个名为 *META-INF/services/com.sun.tools.jconsole.JConsolePlugin* 的提供程序配置文件。这一行指定实现 *com.sun.tools.jconsole.JConsolePlugin* 的类的完全限定类名。

- `-version`

显示发布信息和退出。

- `-Jflag`

将标志传递给运行 `jconsole` 命令的 JVM。

## ~~jvisualvm~~

> @delete 9<br>
> 对 Java 应用程序进行可视化监视，故障排除和配置文件。

# 监控虚拟机

## jps

> 列出目标系统上的已检测 Java 虚拟机（JVM）。此命令是实验性的，不受支持。

> 概要

```bash
# options 命令行选项。
# hostid 应为其生成进程报告的主机的标识符。hostid 可以包含可选组件，这些组件指示通信协议、端口号和其他特定于实现的数据。
jps [options] [hostid]
```

> 描述

`jps` 命令列出目标系统上仪表化的 Java HotSpot VM。该命令仅限于报告具有访问权限的 jvm 上的信息。

如果 `jps` 命令在没有指定 *hostid* 的情况下运行，那么它将在本地主机上搜索插装的 jvm。如果从一个 *hostid* 开始，那么它将使用指定的协议和端口在指定的主机上搜索 jvm。假设 `jstatd` 进程在目标主机上运行。

`jps` 命令为目标系统上找到的每个检测到的 JVM 标识符(*lvmid*)。*lvmid* 通常是(但不一定是)JVM 进程的操作系统进程标识符。由于没有选项，`jps` 列出了每个 Java 应用程序的 *lvmid*，然后是应用程序类名或 jar 文件名的简短形式。类名或 JAR 文件名的简短形式忽略了类的包信息或 JAR 文件路径信息。

`jps` 命令使用 Java 启动程序查找传递给主方法的类名和参数。如果目标 JVM 是用自定义启动程序启动的，那么类或 JAR 文件名以及主方法的参数是不可用的。在本例中，`jps` 命令输出类名或 JAR 文件名以及主方法参数的未知字符串。

`jps` 命令生成的 jvm 列表可以被授予运行该命令的主体的权限所限制。该命令只列出原则具有由特定于操作系统的访问控制机制确定的访问权限的 jvm。

> 选项

`jps` 命令支持许多修改命令输出的选项。这些选项将来可能会更改或删除。

- `-q`

抑制传递给主方法的类名、JAR 文件名和参数的输出，只生成本地 JVM 标识符列表。

- `-m`

显示传递给主方法的参数。对于嵌入式 jvm，输出可能为 null。

- `-l`

显示应用程序的主类的完整包名或应用程序 JAR 文件的完整路径名。

- `-v`

显示传递给 JVM 的参数。

- `-V`

抑制传递给主方法的类名、JAR 文件名和参数的输出，只生成本地 JVM 识符列表。

- `-Joption`

将选项传递给 JVM，其中选项是 Java 应用程序启动程序参考页面中描述的选项之一。例如，`-J-Xms48m` 将启动内存设置为 48 MB。

> 主机标识符

主机标识符或 hostid 是指示目标系统的字符串。hostid 字符串的语法对应于 URI 的语法:

```
[protocol:][[//]hostname][:port][/servername]
```

- `protocol`

通信协议。如果省略了协议，并且没有指定主机名，那么默认协议是特定于平台的、经过优化的本地协议。如果省略了协议并指定了主机名，则默认协议是 *rmi*。

- `hostname`

指示目标主机的主机名或 IP 地址。如果省略了 *hostname* 参数，那么目标主机就是本地主机。

- `port`

与远程服务器通信的默认端口。如果省略 *hostname* 参数或协议参数指定优化的本地 *protocol*，则忽略 *port* 参数。否则，*port* 参数的处理是特定于实现的。对于默认的 *rmi* 协议，*port* 参数指示远程主机上 rmiregistry 的端口号。如果省略 *port* 参数，协议参数指示 *rmi*，则使用默认的 rmiregistry 端口(1099)。

- `servername`

该参数的处理取决于实现。对于优化的本地协议，该字段将被忽略。对于 *rmi* 协议，这个参数是一个字符串，表示远程主机上rmi远程对象的名称。有关更多信息，请参见 `jstatd` 命令 `-n` 选项。

> 输出格式

`jps` 命令的输出遵循以下模式:

```
lvmid [ [ classname | JARfilename | "Unknown"] [ arg* ] [ jvmarg* ] ]
```

所有输出标记都用空格分隔。当试图将参数映射到它们的实际位置参数时，包含嵌入式空白的 *arg* 值会产生歧义。

注意: 建议您不要编写用于解析 `jps` 输出的脚本，因为在将来的版本中格式可能会发生变化。如果您编写了解析 `jps` 输出的脚本，那么就需要为这个工具的未来版本修改它们。

> 例子

列出本地主机上检测到的 jvm:

```bash
jps
18027 Java2Demo.JAR
18032 jps
18005 jstat
```

下面的示例列出了远程主机上检测到的 jvm。本例假设 `jstat` 服务器及其内部 RMI 注册表或单独的外部 rmiregistry 进程正在缺省端口(端口1099)的远程主机上运行。它还假定本地主机具有访问远程主机的适当权限。这个示例还包括 `-l` 选项，用于输出类名或 JAR 文件名的长格式。

```bash
jps -l remote.domain
3002 /opt/jdk1.7.0/demo/jfc/Java2D/Java2Demo.JAR
2857 sun.tools.jstatd.jstatd
```

下面的示例列出远程主机上的检测 jvm，该主机具有 RMI 注册表的非默认端口。本例假设 `jstatd` 服务器在远程主机上运行，并将内部 RMI 注册表绑定到端口 2002。本例还使用 `-m` 选项来包含传递给列出的每个 Java 应用程序的主方法的参数。

```bash
jps -m remote.domain:2002
3002 /opt/jdk1.7.0/demo/jfc/Java2D/Java2Demo.JAR
3102 sun.tools.jstatd.jstatd -p 2002
```

## jstat

> 监视 Java 虚拟机（JVM）统计信息。此命令是实验性的，不受支持。

> 概要

```bash
# generalOption 一个通用命令行选项 -help 或 -options。
# outputOptions 一个或多个输出选项，包括单个 statOption，加上 -t、-h 和 -J 选项中的任何一个。
# vmid 虚拟机标识符，它是一个字符串，指示目标 JVM。一般语法如下:
#      [protocol:][//]lvmid[@hostname[:port]/servername]
#      vmid 字符串的语法对应于 URI 的语法。vmid 字符串可以是表示本地 JVM 的简单整数，也可以是指定通信协议、端口号和其他特定于实现的值的更复杂的构造。
# interval [s|ms] 以指定单位、秒或毫秒为单位的采样间隔。默认单位是毫秒。必须是正整数。当指定时，jstat 命令每隔一段时间产生一次输出。
# count 要显示的样本数量。默认值为无穷大，这会导致 jstat 命令显示统计信息，直到目标 JVM 终止或 jstat 命令终止。这个值必须是正整数。
jstat [generalOption | outputOptions vmid [interval[s|ms] [count]]
```

> 描述

`jstat` 命令显示被检测的 Java HotSpot VM 的性能统计数据。目标 JVM 由其虚拟机标识符或 *vmid* 选项标识。

> 虚拟机标识符

vmid 字符串的语法对应于 URI 的语法:

```
[protocol:][//]lvmid[@hostname[:port]/servername]
```

- `protocol`

通信协议。如果省略了协议值，并且没有指定主机名，则默认协议是特定于平台的优化本地协议。如果省略协议值并指定主机名，则默认协议为 rmi。

- `lvmid`

目标 JVM 的本地虚拟机标识符。*lvmid* 是一个特定于平台的值，它惟一地标识系统上的 JVM。*lvmid* 是虚拟机标识符惟一需要的组件。*lvmid* 通常是(但不一定是)目标 JVM 进程的操作系统进程标识符。您可以使用 `jps` 命令来确定 *lvmid*。此外，您还可以使用 `ps` 命令在 Solaris、Linux 和 OS X 平台上确定 *lvmid*，或使用 Windows 任务管理器在 Windows 上确定 *lvmid*。

- `hostname`

指示目标主机的主机名或 IP 地址。如果省略了 *hostname* 值，那么目标主机就是本地主机。

- `port`

与远程服务器通信的默认端口。如果省略主机名值或协议值指定优化的本地协议，则忽略端口值。否则，端口参数的处理是特定于实现的。对于默认的 rmi 协议，端口值指示远程主机上 rmiregistry 的端口号。如果省略端口值，协议值指示 rmi，则使用缺省的 rmiregistry 端口(1099)。

- `servername`

*servername* 参数的处理取决于实现。对于优化的本地协议，这个字段被忽略。对于 rmi 协议，它表示远程主机上 rmi 远程对象的名称。

> 选项

`jstat` 命令支持两种类型的选项，general 选项和 output 选项。一般选项使 `jstat` 命令显示简单的用法和版本信息。输出选项决定统计输出的内容和格式。

所有选项及其功能都可能在将来的版本中更改或删除。

**通用选项**

如果指定一个常规选项，则不能指定任何其他选项或参数。

- `help`

显示帮助消息。

- `-options`

显示静态选项列表。

**输出选项**

如果没有指定常规选项，则可以指定输出选项。输出选项决定 `jstat` 命令输出的内容和格式，并由一个 *statOption* 和其他输出选项(`-h`、`-t` 和 `-J`)组成。必须首先使用 *statOption*

输出格式为表，列由空格分隔。标题行描述列。使用 `-h` 选项设置显示标题的频率。列标题名称在不同选项之间是一致的。通常，如果两个选项提供具有相同名称的列，则这两个列的数据源是相同的。

使用 `-t` 选项显示时间戳列，将时间戳标记为输出的第一列。Timestamp 列包含目标 JVM 启动以来的运行时间(以秒为单位)。时间戳的分辨率依赖于各种因素，并且由于负载过重的系统上的线程调度延迟，时间戳的分辨率可能会发生变化。

使用 interval 和 count 参数分别确定 `jstat` 命令显示输出的频率和次数。

注意:不要编写脚本来解析 `jstat` 命令的输出，因为在将来的版本中格式可能会发生变化。如果您编写了解析 `jstat` 命令输出的脚本，那么就需要为这个工具的未来版本修改它们。

- `-statOption`

确定 `jstat` 命令显示的统计信息。下面列出了可用的选项。使用`-options` general 选项显示特定平台安装的选项列表。

*class*: 显示有关类加载器行为的统计信息。

*compiler*: 显示有关 Java HotSpot VM 即时编译器行为的统计信息。

*gc*: 显示关于垃圾收集堆的行为的统计信息。

*gccapacity*: 显示有关代的容量及其对应空间的统计信息。

*gccause*: 显示关于垃圾收集统计信息的摘要(与 `-gcutil` 相同)，以及上次和当前(如果适用)垃圾收集事件的原因。

*gcnew*: 显示新一代行为的统计信息。

*gcnewcapacity*: 显示关于新一代的大小及其对应空间的统计信息。

*gcold*: 显示关于老一代行为的统计信息和元空间统计信息。

*gcoldcapacity*: 显示关于老一代大小的统计信息。

*gcmetacapacity*: 显示有关元空间大小的统计信息。

*gcutil*: 显示关于垃圾收集统计信息的摘要。

*printcompilation*: 显示 Java HotSpot VM 编译方法统计信息。

- `-h n`

每 n 个样本(输出行)显示一个列标题，其中 n 是一个正整数。默认值为 0，它显示列标题第一行数据。

- `-t`

将时间戳列显示为输出的第一列。时间戳是目标 JVM 启动时间之后的时间。

- `-JjavaOption`

将 javaOption 传递给 Java 应用程序启动程序。例如，`-J-Xms48m` 将启动内存设置为 48 MB

**统计选项和输出**

以下信息总结了 `jstat` 命令为每个 statOption 输出的列。

- `-class option`

类装入器统计数据。

*Loaded*: 加载的类的数量。

*Bytes*: 加载的 kBs 数量。

*Unloaded*: 卸载的类的数量。

*Bytes*: 卸载的字节数。

*Time*: 用于执行类加载和卸载操作的时间。

- `-compiler option`

Java HotSpot VM 即时编译器统计数据。

*Compiled*: 执行编译任务的数量。

*Failed*: 编译任务失败的次数。

*Invalid*: 无效编译任务的数目。

*Time*: 用于执行编译任务的时间。

*FailedType*: 上次编译失败的编译类型。

*FailedMethod*: 上次编译失败的类名和方法。

- `-gc option`

堆垃圾收集统计数据。

*S0C*: 当前幸存者空间 0 容量(kB)。

*S1C*: 当前幸存者空间 1 容量(kB)。

*S0U*: 幸存者空间 0 利用率(kB)。

*S1U*: 幸存者空间 1 利用率(kB)。

*EC*: 当前伊甸园空间容量(kB)。

*EU*: 伊甸园空间利用(kB)。

*OC*: 当前老空间容量(kB)。

*OU*: 老空间利用率(kB)。

*MC*: Metaspace 容量(kB)。

*MU*: Metacspace 利用率(kB)。

*CCSC*: 压缩类空间容量(kB)。

*CCSU*: 使用的压缩类空间(kB)。

*YGC*: 年轻代垃圾收集事件的数量。

*YGCT*: 年轻一代垃圾收集时间。

*FGC*: Full GC 事件的数量。

*FGCT*: Full 的垃圾收集时间。

*GCT*: 垃圾收集总时间。

- `-gccapacity option`

内存池生成和空间容量。

*NGCMN*: 最低新一代容量(kB)。

*NGCMX*: 最大新一代容量(kB)。

*NGC*: 当前新一代容量(kB)。

*S0C*: 当前幸存者空间 0 容量(kB)。

*S1C*: 当前幸存者空间 1 容量(kB)。

*EC*: 当前伊甸园空间容量(kB)。

*OGCMN*: 最小旧代容量（kB）。

*OGCMX*: 最大老一代容量（kB）。

*OGC*: 当前的旧代容量（kB）。

*OC*: 当前旧空间容量（kB）。

*MCMN*: 最小元空间容量（kB）。

*MCMX*: 最大元空间容量（kB）。

*MC*: 元空间容量（kB）。

*CCSMN*: 压缩类空间最小容量（kB）。

*CCSMX*: 压缩类空间最大容量（kB）。

*CCSC*: 压缩类空间容量（kB）。

*YGC*: 年轻一代 GC 活动的数量。

*FGC*: Full GC 事件的数量。

- `-gccause option`

此选项显示与 `-gcutil` 选项相同的垃圾回收统计信息摘要，但包括上次垃圾回收事件的原因和（如果适用）当前垃圾回收事件的原因。除了为 `-gcutil` 列出的列之外，此选项还添加以下列。

*LGCC*: 上次垃圾收集的原因

*GCC*: 当前垃圾收集的原因

- `-gcnew option`

新一代统计数据。

*S0C*: 当前幸存者空间 0 容量(kB)。

*S1C*: 当前幸存者空间 1 容量(kB)。

*S0U*: 幸存者空间 0 利用率(kB)。

*S1U*: 幸存者空间 1 利用率(kB)。

*TT*: 任期阈值。

*MTT*: 最大的任期阈值。

*DSS*: 所需的幸存者大小(kB)。

*EC*: 当前伊甸园空间容量(kB)。

*EU*: 伊甸园空间利用(kB)。

*YGC*: 年轻一代 GC 事件的数量。

*YGCT*: 年轻一代垃圾收集时间。

- `-gcnewcapacity option`

新一代空间大小统计。

*NGCMN*: 最低新一代容量(kB)。

*NGCMX*: 最大新一代容量(kB)。

*NGC*: 当前新一代容量(kB)。

*S0CMX*: 最大幸存者空间 0 容量(kB)。

*S0C*: 当前幸存者空间 0 容量(kB)。

*S1CMX*: 最大幸存者空间 1 容量(kB)。

*S1C*: 当前幸存者空间 1 容量(kB)。

*ECMX*: 最大伊甸园空间容量(kB)。

*EC*: 当前伊甸园空间容量(kB)。

*YGC*: 年轻一代 GC 事件的数量。

*FGC*: Full GC 事件的数量。

- `-gcold option`

老一代和元空间行为统计。

*MC*: Metaspace 容量（kB）

*MU*: Metaspace 使用量（kB）

*CCSC*: 使用的压缩类空间(kB)。

*OC*: 当前旧空间容量(kB)。

*OU*: 旧空间利用率(kB)。

*YGC*: 年轻一代 GC 事件的数量。

*FGC*: Full GC 事件数量。

*FGCT*: 完全的垃圾收集时间。

*GCT*: 垃圾收集总时间。

- `-gcoldcapacity option`

老一代规模统计。

*OGCMN*: 最小旧代容量（kB）。

*OGCMX*: 最大老一代容量（kB）。

*OGC*: 当前的旧代容量（kB）。

*OC*: 当前旧空间容量(kB)。

*YGC*: 年轻一代 GC 事件的数量。

*FGC*: 完整 GC 事件的数量。

*FGCT*: 完全的垃圾收集时间。

*GCT*: 垃圾收集总时间。

- `-gcmetacapacity option`

Metaspace 大小的统计数据。

*MCMN*: 最小元空间容量(kB)。

*MCMX*: 最大元空间容量(kB)。

*MC*: Metaspace 容量(kB)。

*CCSMN*: 压缩类空间最小容量(kB)。

*CCSMX*: 压缩类空间最大容量(kB)。

*YGC*: 年轻一代 GC 事件的数量。

*FGC*: 完整 GC 事件的数量。

*FGCT*: 完全的垃圾收集时间。

*GCT*: 垃圾收集总时间。

- `-gcutil option`

垃圾收集统计数据摘要。

*S0*: 幸存者空间 0 的利用率占空间当前容量的百分比。

*S1*: 幸存者空间 1 的利用率占空间当前容量的百分比。

*E*: 伊甸园空间利用率占空间当前容量的百分比。

*O*: 旧空间利用率占空间当前容量的百分比。

*M*: 元空间利用率占空间当前容量的百分比。

*CCS*: 压缩类空间利用率的百分比。

*YGC*: 年轻一代 GC 事件的数量。

*YGCT*: 年轻一代垃圾收集时间。

*FGC*: 完整 GC 事件的数量。

*FGCT*: 完全的垃圾收集时间。

*GCT*: 垃圾收集总时间。

- `-printcompilation option`

Java HotSpot VM 编译器方法统计。

*Compiled*: 由最近编译的方法执行的编译任务的数量。

*Size*: 字节数字节码的最新编译方法。

*Type*: 最近编译的方法的编译类型。

*Method*: 类名和方法名，标识最近编译的方法。类名使用斜杠(/)代替点(.)作为名称空间分隔符。方法名是指定类中的方法。这两个字段的格式与HotSpot `-XX:+PrintCompilation` 选项一致。

> 例子

本节给出一些监视 *lvmid* 为 21891 的本地 JVM 的示例。

**gcutil 选项**

这个示例连接到 *lvmid* 21891，以 250 毫秒的间隔获取 7 个样本，并显示由 `-gcutil` 选项指定的输出。

这个示例的输出显示，在第三个和第四个示例之间发生了一个年轻代集合。该集合耗时 0.078 秒，将对象从 eden 空间(E)提升到 old 空间(O)，使 old 空间利用率从 66.80% 提升到 68.19%。收集前的幸存者空间利用率为 97.02%，收集后的幸存者空间利用率为 91.03%。

```bash
jstat -gcutil 21891 250 7
 S0     S1     E      O      M      CCS      YGC    YGCT    FGC    FGCT     GCT   
  0.00  97.02  70.31  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  86.23  66.80  95.52  89.14      7    0.300     0    0.000    0.300
  0.00  97.02  96.53  66.80  95.52  89.14      7    0.300     0    0.000    0.300
 91.03   0.00   1.98  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  15.82  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
 91.03   0.00  17.80  68.19  95.89  91.24      8    0.378     0    0.000    0.378
```

**重复列标题字符串**

这个示例连接到 lvmid 21891，以 250 毫秒的间隔获取样本，并显示 `-gcnew` 选项指定的输出。此外，它使用 `-h3` 选项在每 3 行数据之后输出列标题。

除了显示重复的头字符串外，这个示例还显示在第二个和第三个示例之间发生了一个年轻的 GC。持续时间为 0.001 秒。该集合发现了足够多的活动数据，以致幸存者空间 0 利用率(S0U)将超过所需的幸存者大小(DSS)。结果，对象被提升到老一代(在这个输出中不可见)，并且张紧阈值(TT)从 31 降低到 2。

另一个收集发生在第 5 和第 6 个样本之间。这次收集发现的幸存者非常少，并将紧张的阈值恢复到 31。

```bash
jstat -gcnew -h3 21891 250
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    178.6    249    0.203
  64.0   64.0    0.0   31.7 31  31   32.0    512.0    355.5    249    0.203
  64.0   64.0   35.4    0.0  2  31   32.0    512.0     21.9    250    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    245.9    250    0.204
  64.0   64.0   35.4    0.0  2  31   32.0    512.0    421.1    250    0.204
  64.0   64.0    0.0   19.0 31  31   32.0    512.0     84.4    251    0.204
 S0C    S1C    S0U    S1U   TT MTT  DSS      EC       EU     YGC     YGCT
  64.0   64.0    0.0   19.0 31  31   32.0    512.0    306.7    251    0.204
```

**包括每个样本的时间戳**

这个示例连接到 lvmid 21891，并以 250 毫秒的间隔获取 3 个样本。`-t` 选项用于为第一列中的每个示例生成时间戳。

Timestamp 列以秒为单位报告自目标 JVM 启动以来的运行时间。此外， `-gcoldcapacity` 输出显示随着堆扩展以满足分配或提升需求，旧的生成容量(OGC)和旧的空间容量(OC)会增加。在第 81 次完全垃圾收集(FGC)之后，旧的生成容量(OGC)已经从 11,696 kB 增长到 13,820 kB。生成的最大容量(和空间)是 60,544 kB (OGCMX)，因此它仍然有扩展的空间。

```bash
Timestamp      OGCMN    OGCMX     OGC       OC       YGC   FGC    FGCT    GCT
          150.1   1408.0  60544.0  11696.0  11696.0   194    80    2.874   3.799
          150.4   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
          150.7   1408.0  60544.0  13820.0  13820.0   194    81    2.938   3.863
```

**监视远程 JVM 的检测**

这个示例附加到名为 remote 的系统上的 lvmid 40496。使用 `-gcutil` 选项，每隔一秒无限期地取样一次。

lvmid 与远程主机的名称相结合，构建一个 *40496@remote.domain* 的 *vmid*。此 vmid 导致使用 *rmi* 协议与远程主机上的默认 `jstatd` 服务器通信。`jstatd` 服务器使用远程上的 *rmiregistry* 命令定位。绑定到 *rmiregistry* 命令的默认端口(端口 1099)的域。

```bash
jstat -gcutil 40496@remote.domain 1000
... output omitted
```

## jstatd

> 监视 Java 虚拟机（JVM）并使远程监视工具能够连接到 JVM。此命令是实验性的，不受支持。

> 概述

```bash
# options 命令行选项
jstatd [options]
```

> 描述

`jstatd` 命令是一个 RMI 服务器应用程序，它监视仪表化 Java HotSpot VMs 的创建和终止，并提供一个接口，使远程监视工具能够附加到运行在本地主机上的 JVMs 上。

`jstatd` 服务器需要本地主机上的 RMI 注册表。`jstatd` 服务器尝试在缺省端口或使用 `-p` 端口选项指定的端口上附加到 RMI 注册表。如果没有找到 RMI 注册表，则在 `jstatd` 应用程序中创建一个注册表，该注册表绑定到 `-p` 端口选项所指示的端口，或者在省略 `-p` 端口选项时绑定到默认 RMI 注册表端口。您可以通过指定 `-nr` 选项来停止创建内部 RMI 注册表。

> 选项

- `-nr`

如果没有找到现有的 RMI 注册中心，则不尝试在 `jstatd` 进程中创建内部 RMI 注册中心。

- `-p port`

如果没有指定 `-nr` 选项，则创建 RMI 注册表预期在哪里找到的端口号，或者没有找到时创建的端口号。

- `-n rminame`

远程 RMI 对象在 RMI 注册表中绑定到的名称。默认名称是 *JStatRemoteHost*。如果在同一主机上启动多个 `jstatd` 服务器，则可以通过指定此选项使每个服务器的导出 RMI 对象的名称变得惟一。但是，这样做需要在监视客户机的 *hostid* 和 *vmid* 字符串中包含唯一的服务器名。

- `-Joption`

将选项传递给 JVM，其中选项是 Java 应用程序启动程序参考页面中描述的选项之一。例如，-`J-Xms48m` 将启动内存设置为 48 MB。

> 安全

`jstatd` 服务器只能监视具有适当本机访问权限的 jvm。因此，`jstatd` 进程必须使用与目标 jvm 相同的用户凭据运行。一些用户凭证，例如 Solaris、Linux 和 OS X 操作系统中的根用户，具有访问系统上任何 JVM 导出的检测的权限。使用这种凭证运行的 `jstatd` 进程可以监视系统上的任何 JVM，但是会带来额外的安全问题。

`jstatd` 服务器不提供任何远程客户端的身份验证。因此，运行 `jstatd` 服务器进程将公开所有 jvm 导出的检测结果，其中 `jstatd` 进程对网络上的任何用户都具有访问权限。这种公开在您的环境中可能是不受欢迎的，因此，在启动 `jstatd` 进程之前，应该考虑本地安全策略，特别是在生产环境或不安全的网络上。

下面的策略文件允许 `jstatd` 服务器在没有任何安全异常的情况下运行。与将所有权限授予所有代码库相比，此策略的自由度要小一些，但比授予运行 `jstatd` 服务器的最低权限的策略更大。

```
grant codebase "file:${java.home}/../lib/tools.jar" {   
    permission java.security.AllPermission;
};
```

要使用此策略设置，请将文本复制到名为 *jstat.all.policy* 的文件中。并运行 `jstatd` 服务器如下:

```bash
jstatd -J-Djava.security.policy=jstatd.all.policy
```

对于具有更严格安全实践的站点，可以使用自定义策略文件来限制对特定可信主机或网络的访问，尽管此类技术受到 IP 地址欺骗攻击。如果不能使用定制的策略文件解决安全问题，那么最安全的操作就是不运行 `jstatd` 服务器，而是在本地使用 `jstat` 和 `jps` 工具。

> 远程接口

`jstatd` 进程导出的接口是专有的，并且保证会更改。不鼓励用户和开发人员编写这个接口。

> 例子

下面是 `jstatd` 命令的示例。`jstatd` 脚本在后台自动启动服务器

**内部 RMI 注册表**

这个示例展示了如何使用内部 RMI 注册表启动 `jstatd` 会话。本例假设没有其他服务器绑定到默认的 RMI 注册表端口(端口 1099)。

```bash
jstatd -J-Djava.security.policy=all.policy
```

**外部 RMI 注册表**

本例使用外部 RMI 注册表启动 `jstatd` 会话。

```bash
rmiregistry&
jstatd -J-Djava.security.policy=all.policy
```

本例使用端口 2020 上的外部 RMI 注册服务器启动 `jstatd` 会话。

```bash
jrmiregistry 2020&
jstatd -J-Djava.security.policy=all.policy -p 2020
```

这个示例在端口 2020 上使用一个外部 RMI 注册表启动 `jstatd` 会话，该注册表绑定到 *AlternateJstatdServerName*。

```bash
rmiregistry 2020&
jstatd -J-Djava.security.policy=all.policy -p 2020
    -n AlternateJstatdServerName
```

**停止创建进程内 RMI 注册表**

本例启动一个 `jstatd` 会话，当没有找到 RMI 注册表时，该会话不会创建 RMI 注册表。本例假设 RMI 注册表已经运行。如果 RMI 注册表没有运行，那么将显示一条错误消息。

```bash
jstatd -J-Djava.security.policy=all.policy -nr
```

**启用 RMI 日志记录**

本例启动了启用 RMI 日志功能的 `jstatd` 会话。此技术对于故障诊断或监视服务器活动非常有用。

```bash
jstatd -J-Djava.security.policy=all.policy
    -J-Djava.rmi.server.logCalls=true
```

## ~~jmc~~

> @delete 11<br>
> Java Mission Control 是一个分析，监视和诊断工具套件。

# 故障排除

## jcmd

> 将诊断命令请求发送到正在运行的 Java 虚拟机（JVM）。

> 概要

```bash
jcmd [-l|-h|-help]

jcmd pid|main-class PerfCounter.print

jcmd pid|main-class -f filename

jcmd pid|main-class command[ arguments]
```

> 描述

`jcmd` 实用程序用于将诊断命令请求发送到 JVM。它必须在运行 JVM 的同一台机器上使用，并且具有用于启动 JVM 的相同有效用户和组标识符。

**注意：**

要从远程机器或使用不同的标识符调用诊断命令，可以使用 `com.sun.management.DiagnosticCommandMBean` 接口。

如果没有参数或使用 `-l` 选项运行 `jcmd`，它将打印运行 Java 进程标识符的列表，其中包含用于启动进程的主类和命令行参数。使用 `-h` 或 `-help` 选项运行 `jcmd` 将打印工具的帮助消息。

**注意**：

`jcmd` 实用程序可用于在已经运行的 JVM 中与 Java Flight Recorder (JFR)进行动态交互。您可以使用它来解锁商业功能，启用/启动/停止飞行记录，并从系统中获取各种状态消息。

如果将进程标识符(pid)或主类(main-class)指定为第一个参数，`jcmd` 将诊断命令请求发送给具有指定标识符的 Java 进程，或者发送给具有指定主类名称的所有 Java 进程。还可以通过指定 0 作为流程标识符将诊断命令请求发送到所有可用的 Java流程。使用下列命令之一作为诊断命令请求:

- `Perfcounter.print`

打印指定 Java 进程可用的性能计数器。性能计数器的列表可能会随着 Java 进程而变化。

- `-f filename`

要从中读取诊断命令并将其发送到指定 Java 进程的文件的名称。仅与 `-f` 选项一起使用。文件中的每个命令都必须写在一行上。以数字符号(#)开头的行将被忽略。当读取所有行或包含 stop 关键字的行时，文件的处理结束。

- `command [arguments]`

要发送到指定 Java 进程的命令。通过向该进程发送 *help* 命令，可以获得给定进程的可用诊断命令列表。每个诊断命令都有自己的一组参数。要查看命令的描述、语法和可用参数列表，请使用该命令的名称作为 *help* 命令的参数。

注意:如果任何参数包含空格，必须用单引号或双引号('或")包围它们。此外，必须使用反斜杠(`\`)转义单引号或双引号，以防止操作系统 shell 处理引号。或者，可以用单引号包围这些参数，然后用双引号(或者用双引号，然后用单引号)。

> 选项

选择是相互排斥的。

- `-f filename`

从指定文件中读取命令。只有在将流程标识符或主类指定为第一个参数时，才可以使用此选项。文件中的每个命令都必须写在一行上。以数字符号(#)开头的行将被忽略。当读取所有行或包含 stop 关键字的行时，文件的处理结束。

- `-h`
- `-help`

打印帮助信息。

- `-l`

打印使用主类和命令行参数运行 Java 进程标识符的列表。

## jdb

> 查找并修复 Java 平台程序中的错误。

> 概要

```bash
# options 选项
# classname 要调试的 main 类的名称。
# arguments 传递给类的 main(String[]) 方法的参数。
jdb [options] [classname] [arguments]
```

> 描述

Java 调试器(JDB)是一个用于 Java 类的简单命令行调试器。`jdb` 命令及其选项调用 JDB。`jdb` 命令演示了 Java 平台调试器体系结构(JDBA)，并提供了对本地或远程Java虚拟机(JVM)的检查和调试。

**启动一个 JDB 会话**

启动 JDB 会话的方法有很多。最常用的方法是让 JDB 启动一个新的 JVM，并对应用程序的主类进行调试。通过将命令行中的 `java` 命令替换为 `jdb` 命令来实现这一点。例如，如果您的应用程序的主类是 *MyClass*，那么使用以下命令在 JDB 下调试它:

```bash
jdb MyClass
```

以这种方式启动时，`jdb` 命令使用指定的参数调用第二个 JVM，加载指定的类，并在执行该类的第一条指令之前停止 JVM。

使用 `jdb` 命令的另一种方法是将其附加到已经运行的 JVM 上。当 JVM 运行时，用于启动 `jdb` 命令附加到其中的 JVM 的语法如下。这将加载进程内调试库并指定要建立的连接类型。

```bash
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n MyClass
```

然后，可以使用以下命令将 `jdb` 命令附加到 JVM:

```bash
jdb -attach 8000
```

在本例中，*MyClass* 参数没有在 `jdb` 命令行中指定，因为 `jdb` 命令连接到一个现有 JVM，而不是启动一个新的 JVM。

还有许多其他方法可以将调试器连接到 JVM，所有这些方法都受到 `jdb` 命令的支持。Java 平台调试器体系结构有关于这些连接选项的附加文档。

**基本 jdb 命令**

下面是基本的 `jdb` 命令列表。JDB 支持使用 `-help` 选项列出的其他命令。

- `help or ?`

*help* 还是 *?* 命令显示可识别命令的列表，并提供简短的说明。

- `run`

启动 JDB 并设置断点之后，可以使用 `run` 命令执行调试后的应用程序。 `run` 命令只有在 `jdb` 命令启动调试后的应用程序时才可用，而不是附加到现有 JVM 上。

- `cont`

在断点、异常或步骤之后继续执行已调试的应用程序。

- `print`

显示 Java 对象和基本值。对于原始类型的变量或字段，将打印实际值。对于对象，打印一个简短的描述。查看 dump 命令，了解如何获取关于对象的更多信息。

注意: 要显示局部变量，必须使用 `javac -g` 选项编译包含的类。

`print` 命令支持许多简单的 Java 表达式，包括那些带有方法调用的表达式，例如:

```jdb
print MyClass.myStaticField
print myObj.myInstanceField
print i + j + k (i, j, k are primities and either fields or local variables)
print myObj.myMethod() (if myMethod returns a non-null)
print new java.lang.String("Hello").length()
```

- `dump`

对于基本值，`dump` 命令与 `print` 命令相同。对于对象，`dump` 命令打印对象中定义的每个字段的当前值。包括静态字段和实例字段。`dump` 命令支持与 `print` 命令相同的一组表达式。

- `thread`

选择一个线程作为当前线程。许多 `jdb` 命令都基于当前线程的设置。线程由 threads 命令中描述的线程索引指定。

- `where`

没有参数的 `where` 命令转储当前线程的堆栈。`where all` 命令将当前线程组中的所有线程的堆栈转储到其中。`where threadindex` 命令在何处转储指定线程的堆栈。

如果当前线程通过断点之类的事件或通过 `suspend` 命令被挂起，那么可以使用 `print` 和 `dump` 命令显示本地变量和字段。`up` 和 `down` 命令选择当前堆栈帧是哪个堆栈帧。

**断点**

断点可以在 JDB 中的行号或方法的第一条指令处设置，例如:

命令 `stop at MyClass:22` 在包含 *MyClass* 的源文件的第 22 行第一个指令处设置断点。

命令 `stop in java.lang.String.length` 在方法 `java.lang.String.length` 的开头设置断点。

命令 `stop in MyClass.<clinit>` 使用 `<clinit>` 标识 *MyClass* 的静态初始化代码。

当方法重载时，还必须指定其参数类型，以便为断点选择合适的方法。例如，*MyClass.myMethod(int,java.lang.String)* 或 *MyClass.myMethod()*。

`clear` 命令使用以下语法删除断点: `clear MyClass:45`。使用不带参数的 `clear` 或 `stop` 命令将显示当前设置的所有断点的列表。`cont` 命令将继续执行。

**步进**

`step` 命令将执行推进到下一行，无论它是在当前堆栈帧中，还是在被调用的方法中。下一个命令将执行推进到当前堆栈帧中的下一行。

**异常**

当异常发生时，抛出线程的调用堆栈中没有 *catch* 语句，JVM 通常会打印异常跟踪并退出。但是，当在 JDB 下运行时，控件在抛出错误时返回给 JDB。然后可以使用 `jdb` 命令诊断异常的原因。

使用 *catch* 命令使经过调试的应用程序在其他抛出异常时停止，例如: `catch java.io.FileNotFoundException` 或 `catch mypackage.BigTroubleException`。指定类或子类的任何异常都将在抛出应用程序时停止。

`ignore` 命令抵消了前面 `catch` 命令的效果。`ignore` 命令不会导致被调试的 JVM 忽略特定的异常，而只会忽略调试器。

> 选项

当您在命令行上使用 `jdb` 命令而不是 `java` 命令时，`jdb` 命令接受与 `java` 命令相同的许多选项，包括 `-D`、`-classpath` 和 `-X` 选项。下面的列表包含被 `jdb` 命令接受的其他选项。

- `-help`

显示帮助信息。

- `-sourcepath dir1:dir2: . . .`

使用指定的路径搜索指定路径中的源文件。如果未指定此选项，则使用 dot(.)的默认路径。

- `-attach address`

使用默认连接机制将调试器附加到正在运行的 JVM。

- `-listen address`

等待正在运行的 JVM 使用标准连接器连接到指定地址。

- `-launch`

在启动 JDB 时立即启动已调试的应用程序。`-launch` 选项消除了对 `run` 命令的需要。启动调试后的应用程序，然后在加载初始应用程序类之前停止。此时，您可以设置任何必要的断点，并使用 `cont` 命令继续执行。

- `-listconnectors`

列出此 JVM 中可用的连接器。

- `-connect connector-name:name1=value1`

使用指定的连接器和列出的参数值连接到目标 JVM。

- `-dbgtrace [flags]`

打印用于调试 `jdb` 命令的信息。

- `-tclient`

在 Java HotSpot VM 客户机中运行应用程序。

- `-tserver`

在 Java HotSpot VM 服务器中运行应用程序。

- `-Joption`

将选项传递给 JVM，其中选项是 Java 应用程序启动程序参考页面中描述的选项之一。例如，`-J-Xms48m` 将启动内存设置为 48 MB

> 选项转发给调试器进程

- `-v -verbose[:class|gc|jni]`

打开详细模式。

- `-Dname=value`

设置系统属性。

- `-classpath dir`

列出用冒号分隔的目录，在其中查找类。

- `-Xoption`

非标准目标 JVM 选项。

## jhsdb

> @since 9<br>
> 您可以使用 `jhsdb` 工具附加到 Java 进程或启动事后调试程序，以便从崩溃的 Java 虚拟机（JVM）中分析核心转储的内容。

## jinfo

> 生成配置信息。此命令是实验性的，不受支持。

> 概要

```bash
# option 命令行选项
# pid 进程 ID
jinfo [option] pid

# executable 生成核心转储的 Java 可执行文件。
# core 要打印配置信息的核心文件。
jinfo [option] executable core

# servier-id 当多个调试服务器运行在同一个远程主机上时，可以使用一个可选的惟一ID。
# remote-hostname-or-IP 远程调试服务器主机名或IP地址。看到 jsadebugd
jinfo [option] [servier-id] remote-hostname-or-IP
```

> 描述

`jinfo` 命令打印指定 Java 进程或核心文件或远程调试服务器的 Java 配置信息。配置信息包括 Java 系统属性和 Java 虚拟机(JVM)命令行标志。如果指定的进程在 64 位 JVM 上运行，那么可能需要指定 `-J-d64` 选项，例如: `jinfo -J-d64 -sysprops pid`。

此实用程序不受支持，并且可能在 JDK 的未来版本中不可用。在没有 *dbgen.dll* 的 Windows 系统中，必须安装用于 Windows 的调试工具才能使这些工具工作。*PATH* 环境变量应该包含目标进程使用的 jvm.dll 的位置，或者生成崩溃转储文件的位置。例如，*set PATH=%JDK_HOME%\jre\bin\client;%PATH%*。

> 选项

- `no-option`

同时打印命令行标志和系统属性名称-值对。

- `-flag name`

打印指定命令行标志的名称和值。

- `-flag name=value`

将指定的命令行标志设置为指定的值。

- `-flags`

打印传递给 JVM 的命令行标志。

- `-sysprops`

将 Java 系统属性打印为名称-值对。

- `-h`
- `-help`

打印帮助信息。

## ~~jhat~~

> @delete 9<br>
> 分析 Java 堆。此命令是实验性的，不受支持。

## jmap

> 打印进程，核心文件或远程调试服务器的共享对象内存映射或堆内存详细信息。此命令是实验性的，不受支持。

> 概要

```bash
# options 选项
# pid 进程 ID
jmap [options] pid

# executable 生成核心转储的 Java 可执行文件。
# core 要打印内存映射的核心文件。
jmap [options] executable core

# server-id 当多个调试服务器运行在同一个远程主机上时，可以使用一个可选的惟一 ID。
# remote-hostname-or-IP 远程调试服务器主机名或 IP 地址。jsadebugd
jmap [options] [pid] server-id@] remote-hostname-or-IP
```

> 描述

`jmap` 命令打印指定进程、核心文件或远程调试服务器的共享对象内存映射或堆内存详细信息。如果指定的进程在 64 位 Java 虚拟机(JVM)上运行，那么可能需要指定 `-J-d64` 选项，例如: `jmap -J-d64 -heap pid`。

注意: 此实用程序不受支持，并且可能在 JDK 的未来版本中不可用。在没有 *dbgen .dll* 文件的 Windows 系统上，必须安装用于 Windows 的调试工具才能使这些工具工作。*PATH* 环境变量应该包含目标进程使用的 *jvm.dll* 文件的位置，或者生成崩溃转储文件的位置，例如: `set PATH=%JDK_HOME%\jre\bin\client;%PATH%`。

> 选项

- `<no option>`

当不使用任何选项时，`jmap` 命令打印共享对象映射。对于目标 JVM 中加载的每个共享对象，将打印开始地址、映射大小和共享对象文件的完整路径。这种行为类似于 Oracle Solaris `pmap` 实用程序。

- `-dump:[live,] format=b, file=filename`

将 *hprof* 二进制格式的 Java 堆转储到文件名。*live* 子选项是可选的，但在指定时，仅转储堆中的活动对象。要浏览堆转储，可以使用 `jhat` 命令来读取生成的文件。

- `-finalizerinfo`

打印关于等待结束的对象的信息。

- `-heap`

打印使用的垃圾收集、头配置和生成级堆使用的堆摘要。此外，还打印了实习字符串的数量和大小。

- `-clstats`

打印 Java 堆的类加载器明智的统计信息。对于每个类加载器，将打印它的名称、它的活动程度、地址、父类加载器以及它加载的类的数量和大小。

- `-F`

当 pid 没有响应时，将此选项与 `jmap -dump` 或 `jmap -histo` 选项一起使用。此模式不支持 `live` 子选项。

- `-h`
- `-help`

打印帮助信息。

- `-Jflag`

将 *flag* 传递到运行 `jmap` 命令的 Java 虚拟机。

## ~~jsadebugd~~

> @delete 9<br>
> 附加到 Java 进程或核心文件，并充当调试服务器。此命令是实验性的，不受支持。

## jstack

> 打印 Java 进程，核心文件或远程调试服务器的 Java 线程堆栈跟踪。此命令是实验性的，不受支持。

> 概要

```bash
# options 选项
# pid 进程 ID
jstack [options] pid

# executable 生成核心转储的 Java 可执行文件。
# core 要打印堆栈跟踪的核心文件。
jstack [options] executable core

# server-id 当多个调试服务器运行在同一个远程主机上时，可以使用一个可选的惟一 ID。
# remote-hostname-or-IP 远程调试服务器主机名或IP地址。jsadebugd
jstack [options] [server-id@] remote-hostname-or-IP
```

> 描述

`jstack` 命令打印指定 Java 进程、核心文件或远程调试服务器的 Java 线程的 Java 堆栈跟踪。对于每个 Java 帧，打印完整的类名、方法名、字节码索引(BCI)和行号(如果可用)。使用 `-m` 选项，`jstack` 命令使用程序计数器(PC)打印所有线程的 Java 和本机帧。对于每个本机帧，打印出与 PC 最接近的本机符号(如果可用)。C++ 错误的名称不会被打乱。要解调 C++ 名称，可以将此命令的输出通过管道传输到 *c++filt*。当指定的进程在 64 位 Java 虚拟机上运行时，您可能需要指定 `-J-d64` 选项，例如: `jstack -J-d64 -m pid`。

注意: 此实用程序不受支持，并且可能在 JDK 的未来版本中不可用。在没有 *dbgen .dll* 文件的 Windows 系统中，必须安装用于 Windows 的调试工具，以便这些工具能够工作。*PATH* 环境变量需要包含目标进程使用的 *jvm.dll* 的位置，或者生成崩溃转储文件的位置。例如:

```bash
set PATH=<jdk>\jre\bin\client;%PATH%
```

> 选项

- `-F`

当 `jstack [-l] pid` 不响应时强制堆栈转储。

- `-l`

长清单。打印关于锁的附加信息，如拥有的 `java.util.concurrent` 的同步器。

- `-m`

打印具有 Java 和本机 C/C++ 框架的混合模式堆栈跟踪。

- `-h`
- `-help`

打印帮助信息

> 已知 Bugs

在 mixed 模式堆栈跟踪中，`-m` 选项不能用于远程调试服务器。

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
> 注意：此工具已重命名为 `javapackager`。可能会在将来的发行版中删除 javafxpackager.exe 文件。请更新您的脚本以使用 `javapackager`。<br>
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
