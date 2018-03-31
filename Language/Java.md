[JDK](#JDK)
  - [8](#8)
  - [1.7](#1_7)
  - [1.6](#1_6)
  - [1.5](#1_5)

# JDK

## 8

1. 接口默认方法

```java
interface Foo {
    default void foo() {
        // do something
    }
}
```

2. Lambda 表达式 () -> {}

```java
interface Power {
    int power(int num);
}
Power power = num -> num * num;
int i = power.power(8);
```

3. 方法引用 ::

```java
// java.util.function 包
Consumer<String> consumer = System.out::println;
consumer.accept("Hello Java8");
```

## 1_7

1. switch 可以使用字符串

实际使用的是该 `String` 类的 `hashCode()`

2. 泛型类型自动推断

```java
List<String> list = new ArrayList<>();
```

3. AutoCloseable 

```
try (InputStream is = new FileInputStream("some.txt")) {
    // do something
} catch (Exception e) {
    // do something
}
```

4. 数值常量下划线

```java
int i = 1_000_000;
```

5. catch 域内异常可用 `|` 隔开

```java
try {
    // do something
} catch (IOException | OtherException e) {
    // do something
}
```

## 1_6

1. `Desktop` 类和 `SystemTray` 类 

在JDK6中 ,AWT新增加了两个类:Desktop和SystemTray。 

前者可以用来打开系统默认浏览器浏览指定的URL,打开系统默认邮件客户端给指定的邮箱发邮件,用默认应用程序打开或编辑文件(比如,用记事本打开以txt为后缀名的文件),用系统默认的打印机打印文档;后者可以用来在系统托盘区创建一个托盘程序. 

2. Console

JDK6 中提供了 `java.io.Console` 类专用来访问基于字符的控制台设备. 你的程序如果要与 `Windows` 下的 `cmd` 或者 Linux 下的 Terminal 交互,就可以用 `Console` 类代劳. 但我们不总是能得到可用的 `Console`, 一个 JVM 是否有可用的 `Console` 依赖于底层平台和 JVM 如何被调用. 如果 JVM 是在交互式命令行 (比如 Windows 的 cmd) 中启动的, 并且输入输出没有重定向到另外的地方, 那么就可以得到一个可用的 `Console` 实例. 

## 1_5

1. 自动装箱与拆箱

自动装箱：每当需要一种类型的对象时，这种基本类型就自动地封装到与它相同类型的包装中。

自动拆箱：每当需要一个值时，被装箱对象中的值就被自动地提取出来，没必要再去调用 `intValue()` 和 `doubleValue()` 方法。

Java —— 类的包装器

类型包装器有：`Double`, `Float`, `Long`, `Integer`, `Short`, `Character` 和 `Boolean`

```java
Integer iObj = 1;
int i = Integer.valueOf(1);
```

2. 枚举 enum

```java
public enum Week {
    MON, TUE, WED, THU, FRI, STU, SUN;
}
```

枚举有两个方法: `values()` 和 `valueOf()`

3. 静态导入

`import static`

4. 可变参数

```java
public static void main(String... args) {} // ... 表示可变
```

5. 内省 (Introspector)

内省是Java语言对Bean类属性、事件的一种缺省处理方法。例如类A中有属性name,那我们可以通过getName,setName来得到其值或者设置新 的值。通过getName/setName来访问name属性，这就是默认的规则。Java中提供了一套API用来访问某个属性的getter /setter方法，通过这些API可以使你不需要了解这个规则（但你最好还是要搞清楚），这些API存放于包java.beans中。

一般的做法是通过类Introspector来获取某个对象的BeanInfo信息，然后通过BeanInfo来获取属性的描述器 （PropertyDescriptor），通过这个属性描述器就可以获取某个属性对应的getter/setter方法，然后我们就可以通过反射机制来 调用这些方法。

6. 泛型(Generic) 

```java
List<String> list = new ArrayList<String>();
```

Java 的泛型只会在编译时检查, 实际在运行时都会变为 `List<Object>`

7. For-Each 循环

```java
for (String s : strings) { // 
    // do something
}
```
