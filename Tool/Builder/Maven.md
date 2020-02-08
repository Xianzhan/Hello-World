<!-- TOC -->

- [Maven](#maven)
    - [why](#why)
- [mvn](#mvn)
    - [antrun](#antrun)
    - [archetype](#archetype)
    - [assembly](#assembly)
    - [clean](#clean)
    - [compile](#compile)
    - [dependency](#dependency)
    - [enforcer](#enforcer)
    - [generate-sources](#generate-sources)
    - [help](#help)
    - [install](#install)
    - [jar](#jar)
    - [package](#package)
    - [release](#release)
    - [resources](#resources)
    - [surefire](#surefire)
    - [test](#test)
    - [validate](#validate)
- [依赖](#依赖)
    - [依赖范围](#依赖范围)
    - [依赖调解](#依赖调解)
- [构建](#构建)
- [资源](#资源)

<!-- /TOC -->

# Maven

## why

**解决问题**

1. 如果项目非常庞大，就不适合使用 package 来划分模块，最好是每一个模块对应一个工程，利于分工协作。借助于 maven 就可以将一个项目拆分成多个工程
2. 借助于 maven 我们可以使用统一的规范方式下载 jar 包，规范
3. 自动查找依赖下载

Maven 本质上是一个插件框架, 它的核心并不执行任何具体的构建任务, 所有这些任务都交给插件来完成, 例如编译源代码是有 `maven-compiler-plugin` 完成的.

# mvn

[Maven Plugins](http://maven.apache.org/plugins/index.html)

`-D` 指定参数, 如 `-Dmaven.test.skip=true` 跳过单元测试<br>
`-P` 指定 Profile 配置, 可以用于区分环境<br>
`-e` 显示 maven 运行出错的信息<br>
`-o` 离线执行命令, 即不去远程仓库更新包<br>
`-X` 显示 maven 允许的 debug 信息<br>
`-U` 强制去远程更新 snapshot 的插件或依赖, 默认每天只更新一次<br>

## antrun

[maven-antrun-plugin](http://maven.apache.org/plugins/maven-antrun-plugin/)

maven-antrun-plugin 能让用户在 Maven 项目中运行 Ant 任务。用户可以直接在该插件的配置以 Ant 的方式编写 Target， 然后交给该插件的 `run` 目标去执行。在一些由 Ant 往 Maven 迁移的项目中，该插件尤其有用。此外当你发现需要编写一些自定义程度很高的任务，同时又觉 得 Maven 不够灵活时，也可以以 Ant 的方式实现之。maven-antrun-plugin 的 `run` 目标通常与生命周期绑定运行。

## archetype

[maven-archetype-plugin](http://maven.apache.org/archetype/maven-archetype-plugin/)

Archtype 指项目的骨架，Maven 初学者最开始执行的 Maven 命令可能就是 `mvn archetype:generate`，这实际上就是让 maven-archetype-plugin 生成一个很简单的项目骨架，帮助开发者快速上手。可能也有人看到一些文档写了 `mvn archetype:create`， 但实际上 create 目标已经被弃用了，取而代之的是 generate 目标，该目标使用交互式的方式提示用户输入必要的信息以创建项目，体验更好。 maven-archetype-plugin 还有一些其他目标帮助用户自己定义项目原型，例如你由一个产品需要交付给很多客户进行二次开发，你就可以为 他们提供一个 Archtype，帮助他们快速上手。

创建 maven 项目: <br>
```
mvn archetype:create
    -DgroupId=packageName # 指定 group
    -DartifactId=projectName # 指定 artifact
    -DarchetypeArtifactId=maven-archetype-webapp # 创建 web 项目
    
mvn archetype:generate
```

## assembly

[maven-assembly-plugin](http://maven.apache.org/plugins/maven-assembly-plugin/)

maven-assembly-plugin 的用途是制作项目分发包，该分发包可能包含了项目的可执行文件、源代码、readme、平台脚本等等。 maven-assembly-plugin 支持各种主流的格式如 zip、tar.gz、jar 和 war 等，具体打包哪些文件是高度可控的，例如用户可以 按文件级别的粒度、文件集级别的粒度、模块级别的粒度、以及依赖级别的粒度控制打包，此外，包含和排除配置也是支持的。maven-assembly-plugin 要求用户使用一个名为 assembly.xml 的元数据文件来表述打包，它的 single 目标可以直接在命令行调用，也可以被绑定至生命周期。

## clean 

删除 target 目录文件

```cmd
mvn clean
```

## compile 

编译 main 目录下的文件, 生成 class 文件在 target 目录

```cmd
mvn compile
```

## dependency

[maven-dependency-plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)

maven-dependency-plugin 最大的用途是帮助分析项目依赖，`dependency:list` 能够列出项目最终解析到的依赖列表，`dependency:tree` 能进一步的描绘项目依赖树，`dependency:analyze` 可以告诉你项目依赖潜在的问题，如果你有直接使用到的却未声明的依赖，该目标就会发出警告。maven-dependency-plugin 还有很多目标帮助你操作依赖文件，例如 `dependency:copy-dependencies` 能将项目依赖从本地 Maven 仓库复制到某个特定的文件夹下面。

## enforcer

[maven-enforcer-plugin](http://maven.apache.org/plugins/maven-enforcer-plugin/)

在一个稍大一点的组织或团队中，你无法保证所有成员都熟悉 Maven，那他们做一些比较愚蠢的事情就会变得很正常，例如给项目引入了外部的  SNAPSHOT 依赖而导致构建不稳定，使用了一个与大家不一致的 Maven 版本而经常抱怨构建出现诡异问题。maven-enforcer-plugin 能够帮助你避免之类问题，它允许你创建一系列规则强制大家遵守，包括设定 Java 版本、设定 Maven 版本、禁止某些依赖、禁止 SNAPSHOT 依赖。只要在一个父 POM 配置规则，然后让大家继承，当规则遭到破坏的时候，Maven 就会报错。除了标准的规则之外，你还可以扩展该插 件，编写自己的规则。maven-enforcer-plugin 的 enforce 目标负责检查规则，它默认绑定到生命周期的 validate 阶段。

## generate-sources

生成源码包

```
mvn generate-sources
```

## help

[maven-help-plugin](http://maven.apache.org/plugins/maven-help-plugin/)

maven-help-plugin 是一个小巧的辅助工具，最简单的 `help:system` 可以打印所有可用的环境变量和 Java 系统属性。`help:effective-pom` 和 `help:effective-settings` 最为有用，它们分别打印项目的有效 POM 和有效 settings，有效 POM 是指合并了所有父 POM（包括 Super POM）后的 XML，当你不确定 POM 的某些信息从何而来时，就可以查看有效 POM。有效 settings 同理，特别是当你发现自己配置的 settings.xml 没有生效时，就可以用 help:effective-settings 来验证。此外，maven-help-plugin 的 describe 目标可以帮助你描述任何一个 Maven 插件的信息，还有 all-profiles 目标和 active-profiles 目标帮助查看项目的 Profile。


## install

在本地仓库下安装本项目文件, 使本项目可被其它项目依赖

有时, 当项目依赖其它服务时, 如推送服务, 我们只有该服务提供的 SDK, 那我们需要将 xxx-n.n.n.jar 包安装到我们本地仓库, 则我们需要运行以下命令

```cmd
mvn install:install-file
    -Dfile=文件路径\xxx-n.n.n.jar
    -DgroupId=xxx.xxx
    -DartifactId=xxx
    -Dversion=n.n.n
    -Dpackaging=jar
```

## jar

打 jar 包

```
mvn jar:jar
```

## package

打包生成 pom.xml 描述文件, 默认保存在 target 目录下

```
mvn package
```

## release

[maven-release-plugin](http://maven.apache.org/plugins/maven-release-plugin/)

maven-release-plugin 的用途是帮助自动化项目版本发布，它依赖于 POM 中的 SCM 信息。`release:prepare` 用来准备版本发布，具体的工作包括检查是否有未提交代码、检查是否有 SNAPSHOT 依赖、升级项目的 SNAPSHOT 版本至 RELEASE 版本、为项目打标签等等。`release:perform` 则是签出标签中的 RELEASE 源码，构建并发布。版本发布是非常琐碎的工作，它涉及了各种检查，而且由于该工作仅仅是偶尔需要，因此手动操作很容易遗漏一些细节，maven-release-plugin 让该工作变得非常快速简便，不易出错。maven-release-plugin 的各种目标通常直接在命令行调用，因为版本发布显然不是日常构建生命周期的一部分。

## resources

[maven-resources-plugin](http://maven.apache.org/plugins/maven-resources-plugin/)

为了使项目结构更为清晰，Maven 区别对待 Java 代码文件和资源文件，maven-compiler-plugin 用来编译 Java 代码，maven-resources-plugin 则用来处理资源文件。默认的主资源文件目录是 src/main/resources，很多用户会需要添加额外的资源文件目录，这个时候就可以通过配置 maven-resources-plugin 来实现。此外，资源文件过滤也是 Maven 的一大特性，你可以在资源文件中使用 ${propertyName} 形式的 Maven 属性，然后配置 maven-resources-plugin 开启对资源文件的过滤，之后就可以针对不同环境通过命令行或者 Profile 传入属性的值，以实现更为灵活的构建。

## surefire

[maven-surefire-plugin](http://maven.apache.org/plugins/maven-surefire-plugin/)

可能是由于历史的原因，Maven 2/3 中用于执行测试的插件不是 maven-test-plugin，而是 maven-surefire-plugin。其实大部分时间内，只要你的测试类遵循通用的命令约定（以 Test 结尾、以 TestCase 结尾、或者以 Test 开头），就几乎不用知晓该插件的存在。然而在当你想要跳过测试、排除某些 测试类、或者使用一些 TestNG 特性的时候，了解 maven-surefire-plugin 的一些配置选项就很有用了。例如 `mvn test -Dtest=FooTest` 这样一条命令的效果是仅运行 FooTest 测试类，这是通过控制 maven-surefire-plugin 的 test 参数实现的。

## test

compile 后进行测试

```
mvn test
```

## validate

验证项目是否正确

```
mvn validate
```

# 依赖

Maven 根据坐标才能找到需要的依赖.

## 依赖范围

`compile`: 编译依赖范围. 如果没有指定, 就会默认使用该依赖范围. 使用此依赖范围的 Maven 依赖, 对于编译、测试、运行三种 classpath 都有效. 典型的例子是 spring-core, 在编译、测试和运行的时候都需要使用该依赖.

`test`: 测试依赖范围. 使用此依赖范围的 Maven 依赖, 只对于测试 classpath 有效, 在编译主代码或者运行项目时无法使用此类依赖. 典型的例子就是 JUnit, 它只有在编译测试代码及运行测试的时候才需要.

`provided`: 已提供依赖范围. 使用此依赖范围的 Maven 依赖, 对于编译和测试 classpath 有效, 但在运行时无效. 典型的例子是 servlet-api, 编译和测试项目的时候需要该依赖, 但在运行项目的时候, 由于容器已经提供, 就不需要 Maven 重复地引入一遍.

`runtime`: 运行时依赖范围. 使用此依赖范围的 Maven 依赖, 对于测试和运行 classpath 有效, 但在编译主代码时无效. 典型的例子是 JDBC 驱动实现, 项目主代码的编译只需要 JDK 提供的 JDBC 接口, 只有在执行测试或者运行项目的时候才需要实现上述接口的具体 JDBC 驱动.

`system`: 系统依赖范围. 该依赖与三种 classpath 的关系, 和 provided 依赖范围完全一致. 但是, 使用 system 范围的依赖时必须通过 systemPath 元素显示地指定依赖文件的路径. 由于此类依赖不是通过 Maven 仓库解析的, 而且往往与本机系统绑定, 可能造成构建不可移植, 因此谨慎使用. systemPath 元素可以引用环境变量, 如: 

```xml
<dependency>
	<groupId>javax.sql</groupId>
	<artifactId>jdbc-stdext</artifactId>
	<version>2.0</version>
	<scope>system</scope>
	<systemPath>${java.home}/lib/rt.jar</systemPath>
</dependency>
```
`import`: 导入依赖范围. 该依赖范围不会对三种 classpath 产生实际的影响.

依赖范围(scope)|编译classpath|测试classpath|运行时classpath|例子
:---:|:---:|:---:|:---:|:---:
compile|Y|Y|Y|spring-core
test|-|Y|-|JUnit
provided|Y|Y|-|servlet-api
runtime|-|Y|Y|JDBC 驱动
system|Y|Y|-|本地类库文件

## 依赖调解

1、 原则一: 路径最近者优先

A->B->C->X(1.0)<br>
A->D->X(2.0)<br>
X(2.0) 优先<br>

2、 原则二: 第一声明者优先

A->B->Y(1.0)<br>
A->C->Y(2.0)<br>
Y(1.0) 优先<br>

# 构建

1. clean 清理: 将以前编译得到的旧文件 class 字节码文件和资源文件删除
2. compile 编译: 将 java 源程序编译成 class 字节码文件
3. test 测试: 自动测试, 自动调用 junit 程序
4. report 报告: 程序执行的结果
5. package 打包: 动态 Web 工程打 war 包, java 工程打 jar 包
6. install 安装: Maven 特定的概念, 将打包得到的文件复制到 "仓库" 中的指定位置
7. deploy 部署: 将动态 Web 工程生成的 war 包复制到 Servlet 容器下, 使其可以运行

# 资源

[GitHub://maven](https://github.com/apache/maven)<br>