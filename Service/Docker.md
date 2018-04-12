[Base](#base)

[Dockerfile](#dockerfile)

# Base

```sh
docker run [组织名称]/<镜像名称>:[镜像标签]

# nginx
docker run \
  -d \
  --rm \
  -p 8080:80 \  # 将本机 8080 端口映射到镜像实例内的 80 端口
  -v "$PWD/workspace":/var/www/hello.world \ # 将本机目录映射到镜像 /var/www/hello.world 文件夹
  -v "$PWD/hello.world.conf":/etc/nginx/conf.d/hello.world.conf \
  nginx
```

# Dockerfile

1. # 表注释
2. FROM 指令告诉 Docker 使用哪个镜像作为基础
3. RUN 开头的指令会在创建中运行, 比如安装一个软件包
4. COPY 指令将文件复制到镜像中
5. WORKDIR 指定工作目录
6. CMD/ENTRYPOINT 容器启动执行命令

RUN 和 CMD/ENTRYPOINT 都是执行命令，区别在于 RUN 是在镜像构建过程中执行的，而 CMD/ENTRYPOINT 是在镜像生成实例的时候执行的，类似于 C/C++ 语言的头文件的正常代码的区别。而且后者在一个 Dockerfile 文件中只能有一个存在。CMD/ENTRYPOINT 的区别除了在写法上有区别之外，还有在 docker run 命令后增加 CMD 参数的情况下有区别（CMD会被复写）。一般建议使用 ENTRYPOINT 会更方便点。一个简单的 Node 命令行脚本的 Dockerfile 文件如下：

```Dockerfile
FROM mhart/alpine-node:8.9.3
LABEL maintainer="lizheming <i@imnerd.org>" \
  org.label-schema.name="Drone Wechat Notification" \
  org.label-schema.vendor="lizheming" \
  org.label-schema.schema-version="1.1.0"
WORKDIR /wechat
COPY package.json /wechat/package.json
RUN npm install --production --registry=https://registry.npm.taobao.org
COPY index.js /wechat/index.js
ENTRYPOINT [ "node", "/wechat/index.js" ]
```
