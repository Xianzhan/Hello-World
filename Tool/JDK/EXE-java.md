<!-- TOC -->

- [时区](#时区)
- [资源](#资源)

<!-- /TOC -->

# 时区

```sh
# 环境变量
export TZ="America/Sao_Paulo"

# 启动参数
java -Duser.timezone="Asia/Kolkata" com.company.Main

# 代码设置
System.setProperty("user.timezone", "Asia/Kolkata");
# 或者
TimeZone.setDefault(TimeZone.getTimeZone("Portugal"));
```

# 资源

[17 个常用的 JVM 参数，你都记住了吗！](https://mp.weixin.qq.com/s/SckAMgHX4uXzmOu221202g)<br>
