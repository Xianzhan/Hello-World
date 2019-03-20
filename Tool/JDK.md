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

## ~~javah~~

> @delete 11<br>
> @see [javac](#javac)<br>
> 从 Java 类生成 C 头文件和源文件。

## javap

> 反汇编一个或多个类文件。

## jdeprscan

> @since 9<br>
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

> @delete 9<br>
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
