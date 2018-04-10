[ClassFile](#classfile)

[Native](#native)
  - [Primitive](#primitive)

[Version](#version)
  
[resources](#resources)

# ClassFile

[实例分析JAVA CLASS的文件结构](http://mp.weixin.qq.com/s/WiO97u0cyqpdoEkVOZ6D_Q)

# Native

[openjdk 源码](https://github.com/dmlloyd/openjdk)

## Primitive

C++ | Java
:---:|:---:
`typedef unsigned char   jboolean`| `boolean`
`typedef unsigned short  jchar`|`char`
`typedef short           jshort`|`short`
`typedef float           jfloat`|`float`
`typedef double          jdouble`|`double`

```h
// jni_md.h
#define JNICALL

typedef int jint;
#ifdef _LP64
typedef long jlong;
#else
typedef long long jlong;
#endif

typedef signed char jbyte;

#endif /* !_JAVASOFT_JNI_MD_H_ */


// jni.h
#ifdef __cplusplus

class _jobject {};
class _jclass : public _jobject {};
class _jthrowable : public _jobject {};
class _jstring : public _jobject {};
class _jarray : public _jobject {};
class _jbooleanArray : public _jarray {};
class _jbyteArray : public _jarray {};
class _jcharArray : public _jarray {};
class _jshortArray : public _jarray {};
class _jintArray : public _jarray {};
class _jlongArray : public _jarray {};
class _jfloatArray : public _jarray {};
class _jdoubleArray : public _jarray {};
class _jobjectArray : public _jarray {};

typedef _jobject *jobject;
typedef _jclass *jclass;
typedef _jthrowable *jthrowable;
typedef _jstring *jstring;
typedef _jarray *jarray;
typedef _jbooleanArray *jbooleanArray;
typedef _jbyteArray *jbyteArray;
typedef _jcharArray *jcharArray;
typedef _jshortArray *jshortArray;
typedef _jintArray *jintArray;
typedef _jlongArray *jlongArray;
typedef _jfloatArray *jfloatArray;
typedef _jdoubleArray *jdoubleArray;
typedef _jobjectArray *jobjectArray;

#else

struct _jobject;

typedef struct _jobject *jobject;
typedef jobject jclass;
typedef jobject jthrowable;
typedef jobject jstring;
typedef jobject jarray;
typedef jarray jbooleanArray;
typedef jarray jbyteArray;
typedef jarray jcharArray;
typedef jarray jshortArray;
typedef jarray jintArray;
typedef jarray jlongArray;
typedef jarray jfloatArray;
typedef jarray jdoubleArray;
typedef jarray jobjectArray;

#endif

typedef jobject jweak;

typedef union jvalue {
    jboolean z;
    jbyte    b;
    jchar    c;
    jshort   s;
    jint     i;
    jlong    j;
    jfloat   f;
    jdouble  d;
    jobject  l;
} jvalue;

/*
 * jboolean constants
 */

#define JNI_FALSE 0
#define JNI_TRUE  1
```

# Version

## 10

1. 局部类型推断

```java
public void main(String... args) {
    var s = "Hello World";
    System.out.println(s);
}
```

## 9

1. Jigsaw (模块化)

模块声明示例

```java
module com.mycompany.sample { 
    exports com.mycompany.sample; 
    requires com.mycompany.common; 
    provides com.mycompany.common.DemoService with
        com.mycompany.sample.DemoServiceImpl; 
}
```

2. jshell (REPL)

```bat
jshell> int i = 9;
jshell> System.out.println(i);
9
```

3. 不可变集合

```java
List.of();
Set.of();
Map.of();
```

4. ProcessHandler 接口

可以对原生进程进行管理，尤其适合于管理长时间运行的进程。在使用 P rocessBuilder 来启动一个进程之后，可以通过 Process.toHandle()方法来得到一个 ProcessHandl e 对象的实例。

```java
final ProcessBuilder processBuilder = new ProcessBuilder("top") 
    .inheritIO(); 
final ProcessHandle processHandle = processBuilder.start().toHandle(); 
processHandle.onExit().whenCompleteAsync((handle, throwable) -> { 
    if (throwable == null) { 
        System.out.println(handle.pid()); 
    } else { 
        throwable.printStackTrace(); 
    } 
});
```

5. 平台日志 API 和 服务

Java 9 允许为 JDK 和应用配置同样的日志实现。新增的 System.LoggerFinder 用来管理 JDK 使 用的日志记录器实现。JVM 在运行时只有一个系统范围的 LoggerFinder 实例。LoggerFinder 通 过服务查找机制来加载日志记录器实现。默认情况下，JDK 使用 java.logging 模块中的 java.util.logging 实现。通过 LoggerFinder 的 getLogger()方法就可以获取到表示日志记录器的 System.Logger 实现。应用同样可以使用 System.Logger 来记录日志。这样就保证了 JDK 和应用使用同样的日志实现。我们也可以通过添加自己的 System.LoggerFinder 实现来让 JDK 和应用使用 SLF4J 等其他日志记录框架。 代码清单 9 中给出了平台日志 API 的使用示例。

```java
public class Main { 
    private static final System.Logger LOGGER = System.getLogger("Main"); 
    public static void main(final String[] args) { 
        LOGGER.log(Level.INFO, "Run!");
    } 
}
```

6. 反应式流 (Reactive Streams)

反应式编程的思想最近得到了广泛的流行。 在 Java 平台上有流行的反应式 库 RxJava 和 R eactor。反应式流规范的出发点是提供一个带非阻塞负压（ non-blocking backpressure ） 的异步流处理规范。反应式流规范的核心接口已经添加到了 Java9 中的 java.util.concurrent.Flow 类中。

Flow 中包含了 Flow.Publisher、Flow.Subscriber、Flow.Subscription 和 F low.Processor 等 4 个核心接口。Java 9 还提供了 SubmissionPublisher 作为 Flow.Publisher 的一个实现。RxJava 2 和 Reactor 都可以很方便的 与 Flow 类的核心接口进行互操作。

7. 并发 

在并发方面，类 CompletableFuture 中增加了几个新的方法。completeAsync 使用一个异步任务来获取结果并完成该 CompletableFuture。orTimeout 在 CompletableFuture 没有在给定的超时时间之前完成，使用 TimeoutException 异常来完成 CompletableFuture。completeOnTimeout 与 o rTimeout 类似，只不过它在超时时使用给定的值来完成 CompletableFuture。新的 Thread.onSpinWai t 方法在当前线程需要使用忙循环来等待时，可以提高等待的效率。

8. Nashorn (ES6 引擎)

Nashorn 是 Java 8 中引入的新的 JavaScript 引擎。Java 9 中的 Nashorn 已经实现了一些 ECMAScript 6 规范中的新特性，包括模板字符串、二进制和八进制字面量、迭代器 和 for..of 循环和箭头函数等。Nashorn 还提供了 API 把 ECMAScript 源代码解析成抽象语法树（ Abstract Syntax Tree，AST ） ，可以用来对 ECMAScript 源代码进行分析。

9. 安全

Java 9 新增了 4 个 SHA- 3 哈希算法，SHA3-224、SHA3-256、SHA3-384 和 S HA3-512。另外也增加了通过 java.security.SecureRandom 生成使用 DRBG 算法的强随机数。 代码清单 13 中给出了 SHA-3 哈希算法的使用示例。

```java
import org.apache.commons.codec.binary.Hex; 
public class SHA3 { 
    public static void main(final String[] args) throws NoSuchAlgorithmException { 
        final MessageDigest instance = MessageDigest.getInstance("SHA3-224"); 
        final byte[] digest = instance.digest("".getBytes()); 
        System.out.println(Hex.encodeHexString(digest)); 
    } 
}
```

10. 接口私有方法

```java
interface Foo {
    private foo() {
        // do something
    }
}
```

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

4. Optional

NullPointerException 

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

# resources

[自己关于VM的帖的目录 - RednaxelaFX](http://rednaxelafx.iteye.com/blog/362738)

[关于Jvm知识看这一篇就够了 - 纯洁的微笑](https://zhuanlan.zhihu.com/p/34426768)
