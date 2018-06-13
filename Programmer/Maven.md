[mvn](#mvn)
  - [clean](#clean)
  - [compile](#compile)
  - [generate-sources](#generate-sources)
  - [install](#install)
  - [jar](#jar)
  - [package](#package)
  - [test](#test)
  - [validate](#validate)

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
