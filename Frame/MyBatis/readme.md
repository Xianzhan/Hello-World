<!-- TOC -->

- [设计模式](#设计模式)
- [插件](#插件)

<!-- /TOC -->

# 设计模式

1. Builder 模式，例如 `SqlSessionFactoryBuilder`、`XMLConfigBuilder`、`XMLMapperBuilder`、`XMLStatementBuilder`、`CacheBuilder`；
2. 工厂模式，例如 `SqlSessionFactory`、`ObjectFactory`、`MapperProxyFactory`；
3. 单例模式，例如 `ErrorContext` 和 `LogFactory`；
4. 代理模式，*Mybatis* 实现的核心，比如 `MapperProxy`、`ConnectionLogger`，用的 JDK 的动态代理；还有 executor.loader 包使用了 cglib 或者 javassist 达到延迟加载的效果；
5. 组合模式，例如 `SqlNode` 和各个子类 `ChooseSqlNode` 等；
6. 模板方法模式，例如 `BaseExecutor` 和 `SimpleExecutor`，还有 `BaseTypeHandler` 和所有的子类例如 `IntegerTypeHandler`；
7. 适配器模式，例如 `Log` 的 *Mybatis* 接口和它对 JDBC、log4j 等各种日志框架的适配实现；
8. 装饰者模式，例如 Cache 包中的 cache.decorators 子包中等各个装饰者的实现；
9. 迭代器模式，例如迭代器模式 `PropertyTokenizer`；

# 插件

[Github://mybatis-generator-plugin](https://github.com/itfsw/mybatis-generator-plugin)<br>
[Mybatis 使用的 9 种设计模式](https://mp.weixin.qq.com/s/1SJA08n5TxS7nTEds-Cu1g)<br>