[Base](#base)

# Base

## 配置

- 本地仓库路径

gradle 的默认仓库路径为用户目录下的 .gradle 目录, gradle 并没有像 maven 那样提供配置文件，若要修改默认仓库路径，我们可以设置环境变量 GRADLE_USER_HOME，windows 下同理。我把仓库设置在 gradle 目录下

Linux:

```sh
export GRADLE_HOME=/usr/local/gradle
export PATH=$GRADLE_HOME/bin:$PATH
export GRADLE_USER_HOME=$GRADLE_HOME/.gradle
```

Windows:

```bat
GRADLE_USER_HOME=c:/gradle/.gradle
```
