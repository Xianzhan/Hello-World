[mvn](#mvn)
  - [clean](#clean)
  - [compile](#compile)
  - [generate-sources](#generate-sources)
  - [install](#install)
  - [jar](#jar)
  - [package](#package)
  - [test](#test)
  - [validate](#validate)

[依赖](#依赖)
 - [依赖范围](#依赖范围)
 - [依赖调解](#依赖调解)

# mvn

`-D` 指定参数, 如 `-Dmaven.test.skip=true` 跳过单元测试<br>
`-P` 指定 Profile 配置, 可以用于区分环境<br>
`-e` 显示 maven 运行出错的信息<br>
`-o` 离线执行命令, 即不去远程仓库更新包<br>
`-X` 显示 maven 允许的 debug 信息<br>
`-U` 强制去远程更新 snapshot 的插件或依赖, 默认每天只更新一次<br>

创建 maven 项目: <br>
```
mvn archetype:create
    -DgroupId=packageName # 指定 group
    -DartifactId=projectName # 指定 artifact
    -DarchetypeArtifactId=maven-archetype-webapp # 创建 web 项目
    
mvn archetype:generate
```

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

## generate-sources

生成源码包

```
mvn generate-sources
```

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
|依赖范围(scope)|编译classpath|测试classpath|运行时classpath|例子|
|:---:|:---:|:---:|:---:|:---|
|compile|Y|Y|Y|spring-core|
|test|-|Y|-|JUnit|
|provided|Y|Y|-|servlet-api|
|runtime|-|Y|Y|JDBC 驱动|
|system|Y|Y|-|本地类库文件|

## 依赖调解

1、 原则一: 路径最近者优先

A->B->C->X(1.0)
A->D->X(2.0)
X(2.0) 优先

2、 原则二: 第一声明者优先

A->B->Y(1.0)
A->C->Y(2.0)
Y(1.0) 优先