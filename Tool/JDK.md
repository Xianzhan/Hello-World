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

> @since 10<br>
> 您可以使用 `jlink` 工具将一组模块及其依赖项组合和优化到自定义运行时映像中。

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
