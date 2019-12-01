<!-- TOC -->

- [JDBC](#jdbc)

<!-- /TOC -->

> @since 1.4

# JDBC

**基于 JDBC 技术的驱动程序可以做三件事**

1. 建立与数据源的连接
2. 将查询和更新语句发送到数据源
3. 处理结果

**所在包**

JDBC API 主要位于 JDK 中的 `java.sql` 包中，之后扩展的内容位于 `javax.sql` 包中，主要包括：

- `class DriverManager`: 负责加载各种不同驱动程序（Driver），并根据不同的请求，向调用者返回相应的数据库连接（Connection）。
- `interface Driver`: 驱动程序，会将自身加载到DriverManager中去，并处理相应的请求并返回相应的数据库连接（Connection）。
- `interface Connection`: 数据库连接，负责进行与数据库间的通讯，SQL执行以及事务处理都是在某个特定Connection环境中进行的。可以产生用以执行SQL的Statement。
- `interface Statement`: 用以执行SQL查询和更新（针对静态SQL语句和单次执行）。
- `interface PreparedStatement`: 用以执行包含动态参数的SQL查询和更新（在服务器端编译，允许重复执行以提高效率）。
- `interface CallableStatement`: 用以调用数据库中的存储过程。
- `class SQLException`: 代表在数据库连接的创建和关闭和SQL语句的执行过程中发生了例外情况（即错误）。

从根本上来说, JDBC 是一种规范，它提供了一套完整的接口，允许对底层数据库进行可移植的访问，Java 可以用于编写不同类型的可执行文件。

JDBC 执行流程

```shell
     开始
      ↓
   注册驱动
 (仅仅做一次)
      ↓
   建立连接
  Connection
      ↓
创建运行 SQL 语句
   Statement
      ↓
    运行语句
      ↓
 处理运行结果
      ↓
   释放资源
      ↓
     结束
```

1. 连接数据源
2. 为数据库传递查询和更新指令
3. 处理数据库响应并返回的结果