[mvn](#mvn)
  - [clean](#clean)
  - [compile](#compile)
  - [install](#install)
    - [jar](#jar)
  - [package](#package)
  - [test](#test)

# mvn



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

## install

在本地仓库下安装本项目文件, 使本项目可被其它项目依赖

### jar

有时, 当项目依赖其它服务时, 如推送服务, 我们只有该服务提供的 SDK, 那我们需要将 xxx-n.n.n.jar 包安装到我们本地仓库, 则我们需要运行以下命令

```cmd
mvn install:install-file
    -Dfile=文件路径\xxx-n.n.n.jar
    -DgroupId=xxx.xxx
    -DartifactId=xxx
    -Dversion=n.n.n
    -Dpackaging=jar
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
