<!-- TOC -->

- [基础](#基础)
    - [面向对象](#面向对象)
        - [接口(interface)](#接口interface)
        - [包(package)](#包package)
    - [变量](#变量)
    - [原始类型](#原始类型)
    - [控制语句](#控制语句)
    - [字符串](#字符串)
    - [泛型](#泛型)
    - [注解](#注解)
    - [异常](#异常)
    - [文件](#文件)
        - [resources](#resources)
    - [JDBC](#jdbc)
    - [多线程](#多线程)
        - [ThreadLocal](#threadlocal)
        - [J.U.C-AQS](#juc-aqs)
    - [反射](#反射)
    - [代理](#代理)
    - [引用](#引用)
    - [Lambda](#lambda)
    - [方法引用](#方法引用)
    - [其它](#其它)
        - [Unsafe](#unsafe)
- [设计模式](#设计模式)
- [资源](#资源)

<!-- /TOC -->

# 基础

**修饰符**

-|-|-
:--:|:---:|:---:
`public`|`protected`|`private`
`abstract`|`static`|`final`
`transient`|`volatile`|`synchronized`
`native`| |

**声明**

-|-|-
:---:|:---:|:---:
`class`|`interface`|`extends`
`package`|`throws`|`implements`
`enum`(5)| |

**原始类型**

-|-|-
:---:|:---:|:---:
`boolean`|`byte`|`char`
`short`|`int`|`long`
`float`|`double`|

**控制流**

-|-|-
:---:|:---:|:---:
`if`|`else`|
`try`|`catch`|`finally`
`do`|`while`|
`for`|`continue`|
`switch`|`case`|`default`
`break`|`throw`|`return`

**其它**

-|-|-
:---:|:---:|:---:
`this`|`new`|`super`
`import`|`instanceof`|`void`
`strictfp`|`assert`|`_`(9)

**禁止使用**

-|-|-
:---:|:---:|:---:
`goto`|`const`|

字面值: `true`, `false`, `null`<br>
标识符：`var`(10)

**注释**

`//` & `/**/`

```java
public static void main(String[] args){
    // \u000d System.out.println("Hello World!");
}
// output: Hello World!
```

## 面向对象

1. 封装

Java 的访问权限控制, 是一种封装的体现

-|类内部|本包|子类|外部包
:---:|:---:|:---:|:---:|:---:
`public`|✔|✔|✔|✔|✔
`protected`|✔|✔|✔|✖
`default`|✔|✔|✖|✖
`private`|✔|✖|✖|✖

2. 继承

Java 的继承只能单继承, `extends`

3. 多态

多态一般可以分为两种, 一种是 overwrite, 另一种是 override.

### 接口(interface)

- 不可实例化, 需类实现

```java
interface Interface {
    // 接口只有常量
    // public static final
    int CONSTANT = 1;

    // public abstract
    void method();

    // @since 8 默认方法
    default void defaultMethod() {
        System.out.println("default");
    }

    // @since 8 静态方法
    static void staticMethod() {
        System.out.println("static");
    }

    // @since 9 私有方法
    private void privateMethod() {
        System.out.println("private");
    }

    // @since 9 私有静态方法
    private static void privateStaticMethod() {
        System.out.println("private static");
    }
}
```

### 包(package)

包是一个**名称空间**，用于以逻辑方式组织类和接口。将代码放入包中可以使大型软件项目更容易管理。

```java
// 默认导入的包, 可不显示导入
import java.lang.*

// @since 5 静态导入
import static java.lang.System.out;
```

## 变量

```java
// @since 10 "局部变量" 的类型推断 var
var list = new ArrayList<String>();
```

## 原始类型

类型|默认值|包装类|虚拟机内部符号
:---:|:---:|:---:|:---:
`boolean`|false|Boolean|Z
`byte`|0|Byte|B
`short`|0|Short|S
`char`|`\u0000`|Character|C
`int`|0|Integer|I
`long`|0L|Long|J
`float`|+0.0F|Float|F
`double`|+0.0D|Double|D

```java
// @since 5 自动装箱拆箱
Integer boxing = 0; // 装箱
int unboxing = box; // 拆箱

// @since 7 下划线
int bin = 0b0111_1111_1111_1111_1111_1111_1111_1111;
int oct = 0177_7777_7777; // 八进制不推荐使用
int dec = 2_147_483_647;
int hex = 0x7f_ff_ff_ff;  // 十六进制每两位表一个字节
```

## 控制语句

```java
// switch 选择
// 支持 byte, char, short, int
int value = 0;
switch (value) {
    case 0:
        // do something
        break;
    default:
        break;
}
// @since 7 String 类型支持
String v = "hello";
switch (v) {
    case "hello":
        // do
        break;
    default:
        break;
}
// @since 12 JEP 325: Switch Expressions (Preview)
switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> System.out.println(6);
    case TUESDAY                -> System.out.println(7);
    case THURSDAY, SATURDAY     -> System.out.println(8);
    case WEDNESDAY              -> System.out.println(9);
}

// for 循环
int[] array = {0, 1, 2, 3};
for (int i : array) {
    // @since 5 foreach 循环
    System.out.println(i);
}
```

[JEP 325: Switch Expressions (Preview)](http://openjdk.java.net/jeps/325)<br>

## 字符串

```java
public final class String {
    private final char[] value;

    // @since 9 value 类型由 char[] -> byte[]
    private final byte[] value;
}
```

[一个Java字符串中到底有多少个字符?](https://colobu.com/2019/01/04/how-many-charactors-in-a-java-string/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)<br>

## 泛型

> @since 5

类型擦除: 指的就是 Java 源码中的范型信息只允许停留在编译前期，而编译后的字节码文件中将不再保留任何的范型信息。也就是说，范型信息在编译时将会被全部删除，其中范型类型的类型参数则会被替换为 `Object` 类型，并在实际使用时强制转换为指定的目标数据类型。而 C++ 中的模板则会在编译时将模板类型中的类型参数根据所传递的指定数据类型生成相对应的目标代码。

```java
List<Integer> nums = new ArrayList<Integer>();

// @since 7 泛型推断
List<Integer> nums = new ArrayList<>();
```

## 注解

> @since 5

`java.lang.annotation`

> @since 8 重复注解 `@Repeatable`

## 异常

```
Throwable
 → Error
 → Exception
    → ClassNotFoundException
    → CloneNotSupportedException
    → IOException
       → EOFException
       → FileNotFoundException
       → MalformedURLException
       → UnknownHostException
    → RuntimeException
       → ArithmeticException
       → ClassCastException
       → IllegalArgumentException
       → IllegalStateException
       → IndexOutOfBoundsException
       → NoSuchElementException
       → NullPointerException
```

```java
// @since 7 捕获多个异常
try {
    Integer.parseInt("hello");
} catch (NumberFormatException | RuntimeException e) {
    // e
}

// @since 7 try-with-resources
try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
    // do
}
```

## 文件

计算机文件（或称文件、电脑档案、档案），是存储在某种长期储存设备或临时存储设备中的一段数据流，并且归属于计算机文件系统管理之下。

```java
// @since 7 Path, Paths
// `C:\development\project\java\misc>java Main`
Path root = Paths.get("/");
System.out.println(root.toAbsolutePath());
// Output: C:\

Path path = Paths.get("");
System.out.println(path.toAbsolutePath());
// Output: C:\development\project\java\misc
```

### resources

在 Java 的世界里，经常需要加载各种资源，如 Spring 框架的 `application.xml`，或者是 web 项目的 `web.xml`。

```java
ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
// 包所在的根目录
URL loaderResource = classLoader.getResource("resource.txt");

// 包所在的根目录
URL classResource = Resources.class.getResource("/resource.txt");
System.out.println(Objects.equals(loaderResource, classResource));
// Output: true

// 该类所在的包目录
URL classResource = Resources.class.getResource("");
```

## JDBC

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

## 多线程

三大核心

1. **原子性**

原子性就和数据库事务的原子性差不多，一个操作中要么全部执行成功或者失败。

2. **可见性**

现代计算机中，由于 CPU 直接从主内存中读取数据的效率不高，所以都会对应的 CPU 高速缓存，先将主内存中的数据读取到缓存中，线程修改数据之后首先更新到缓存，之后才会更新到主内存。如果此时还没有将数据更新到主内存其他的线程此时来读取就是修改之前的数据。

3. **顺序性**

JVM 为了提高整体的效率，在保证最终结果和代码顺序执行结果一致的情况下可能进行指令重排。

[Java 中15种锁的介绍](https://mp.weixin.qq.com/s/qWhcgKxrWz0ei_pKlSynpA)<br>
[Java 多线程-线程池 ThreadPoolExecutor 构造方法和规则](https://blog.csdn.net/qq_25806863/article/details/71126867)<br>

### ThreadLocal

[ThreadLocal的使用及原理分析](https://mp.weixin.qq.com/s/bxIkMaCQ0PriZtSWT8wrXw)<br>

### J.U.C-AQS

`java.util.concurrent` (J.U.C) 大大提高了并发性能，AQS 被认为是 J.U.C 的核心。

**CountdownLatch**

用来控制一个线程等待多个线程。

维护了一个计数器 cnt，每次调用 `countDown()` 方法会让计数器的值减 1，减到 0 的时候，那些因为调用 `await()` 方法而在等待的线程就会被唤醒。

```java
public class CountdownLatchExample {
    public static void main(String[] args) throws InterruptedException {
        final int totalThread = 10;
        CountDownLatch countDownLatch = new CountDownLatch(totalThread);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalThread; i++) {
            executorService.execute(() -> {
                System.out.print("run..");
                countDownLatch.countDown();
            });
        }
        countDownLatch.await();
        System.out.println("end");
        executorService.shutdown();
    }
}
// Output: run..run..run..run..run..run..run..run..run..run..end
```

**CyclicBarrier**

用来控制多个线程互相等待，只有当多个线程都到达时，这些线程才会继续执行。

和 `CountdownLatch` 相似，都是通过维护计数器来实现的。线程执行 `await()` 方法之后计数器会减 1，并进行等待，直到计数器为 0，所有调用 `await()` 方法而在等待的线程才能继续执行。

`CyclicBarrier` 和 `CountdownLatch` 的一个区别是，`CyclicBarrier` 的计数器通过调用 `reset()` 方法可以循环使用，所以它才叫做循环屏障。

`CyclicBarrier` 有两个构造函数，其中 parties 指示计数器的初始值，barrierAction 在所有线程都到达屏障的时候会执行一次。

```java
public CyclicBarrier(int parties, Runnable barrierAction) {
    if (parties <= 0) throw new IllegalArgumentException();
    this.parties = parties;
    this.count = parties;
    this.barrierCommand = barrierAction;
}

public CyclicBarrier(int parties) {
    this(parties, null);
}
```

```java
public class CyclicBarrierExample {
    public static void main(String[] args) {
        final int totalThread = 10;
        CyclicBarrier cyclicBarrier = new CyclicBarrier(totalThread);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalThread; i++) {
            executorService.execute(() -> {
                System.out.print("before..");
                try {
                    cyclicBarrier.await();
                } catch (InterruptedException | BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.print("after..");
            });
        }
        executorService.shutdown();
    }
}
// Output: before..before..before..before..before..before..before..before..before..before..after..after..after..after..after..after..after..after..after..after..
```

**Semaphore**

`Semaphore` 类似于操作系统中的信号量，可以控制对互斥资源的访问线程数。

以下代码模拟了对某个服务的并发请求，每次只能有 3 个客户端同时访问，请求总数为 10。

```java
public class SemaphoreExample {
    public static void main(String[] args) {
        final int clientCount = 3;
        final int totalRequestCount = 10;
        Semaphore semaphore = new Semaphore(clientCount);
        ExecutorService executorService = Executors.newCachedThreadPool();
        for (int i = 0; i < totalRequestCount; i++) {
            executorService.execute(()->{
                try {
                    semaphore.acquire();
                    System.out.print(semaphore.availablePermits() + " ");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
            });
        }
        executorService.shutdown();
    }
}
// Output: 2 1 2 2 2 2 2 1 2 2
```

## 反射

反射是 Java 语言中一个相当重要的特性，它允许正在运行的 Java 程序观测，甚至是修改程序的动态行为。

反射在 Java 中的应用十分广泛。开发人员日常接触到的 IDE 便运用了这一功能，当然，大部分使用语法树来实现。

## 代理

[动态代理、CGlib、AOP](https://mp.weixin.qq.com/s/lR2pJTy5cbX43YvaQ8uUgQ)<br>

## 引用

[Reference 完全解读](https://www.cnblogs.com/sanzao/p/10343166.html)<br>

## Lambda

> @since 8

Lambda 允许把函数作为一个方法的参数，或者把代码看成数据。

```java
Runnable run = () -> System.out.println("hello");
```

## 方法引用

直接引用已有 Java 类或对象的方法。一般有四种不同的方法引用:

1. 构造器引用。语法是 `Class::new`，或者更一般的 `Class<T>::new`，要求构造器方法是没有参数；
2. 静态方法引用。语法是 `Class::static_method`，要求接受一个 Class 类型的参数；
3. 特定类的任意对象方法引用。它的语法是 `Class::method`。要求方法是没有参数的；
4. 特定对象的方法引用，它的语法是 `instance::method`。要求方法接受一个参数，与 3 不同的地方在于，3 是在列表元素上分别调用方法，而 4 是在某个对象上调用方法，将列表元素作为参数传入；

## 其它

### Unsafe

[【基本功】Java魔法类：Unsafe应用解析](https://mp.weixin.qq.com/s/h3MB8p0sEA7VnrMXFq9NBA)<br>

# 设计模式

[深入理解Java的三种工厂模式](https://mp.weixin.qq.com/s/3R42RC26wRq-xscuNEHc6g)<br>

# 资源

[GitHub://Effective-Java-3rd-edition-Chinese-English-bilingual](https://github.com/clxering/Effective-Java-3rd-edition-Chinese-English-bilingual)<br>
[GitHub://Java-Summarize](https://github.com/zaiyunduan123/Java-Summarize)<br>
[Github://advanced-java](https://github.com/doocs/advanced-java)<br>
[有点长的 Java API 设计清单](https://mp.weixin.qq.com/s/RYBJXQKLJ4guqvz8vXZN9Q)<br>
[我的java问题排查工具单](https://mp.weixin.qq.com/s/nMdBYZjVmF8Tqz6KJLeAaQ)<br>
