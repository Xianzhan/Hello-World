<!-- TOC -->

- [Unsafe](#unsafe)
    - [基本介绍](#基本介绍)
    - [内存操作](#内存操作)
    - [CAS](#cas)
    - [线程调度](#线程调度)
    - [Class 相关](#class-相关)
    - [对象操作](#对象操作)
    - [数组相关](#数组相关)
    - [内存屏障](#内存屏障)
    - [系统相关](#系统相关)
- [FFI](#ffi)

<!-- /TOC -->

# Unsafe

`sun.misc.Unsafe` 提供一些用于执行低级别、不安全的操作的方法, 如直接访问系统内存资源、自主管理内存等资源, 这些方法在提升 Java 运行效率、增强 Java 语言底层资源操作能力方面起到了很大的作用.

## 基本介绍

Unsafe 类是一个单例, 提供静态方法 `getUnsafe()` 获取 Unsafe 实例, 且只有调用 `getUnsafe()` 方法的类为引导类加载器所加载时才合法, 否则抛出 `SecurityException` 异常.

```java
public final class Unsafe {
    
    private Unsafe() {}
    private static final Unsafe theUnsafe = new Unsafe();

    @CallerSensitive
    public static Unsafe getUnsafe() {
        Class<?> caller = Reflection.getCallerClass();
        if (!VM.isSystemDomainLoader(caller.getClassLoader()))
            throw new SecurityException("Unsafe");
        return theUnsafe;
    }
}
```

而我们无法直接使用引导类加载器, 那我们该如何获取这个单例呢? 答案是反射.

```java
private static sun.misc.Unsafe unsafe;

private static Unsafe reflectGetUnsafe() {
    try {
        Class<sun.misc.Unsafe> clazz = sun.misc.Unsafe.class;
        Field theUnsafe = clazz.getDeclaredField(THE_UNSAFE);
        theUnsafe.setAccessible(true);
        unsafe = (sun.misc.Unsafe) theUnsafe.get(null);
    } catch (NoSuchFieldException | IllegalAccessException e) {
        throw new Error("无法初始化 Unsafe");
    }
}
```

## 内存操作

```java
// 分配内存, 相当 malloc() 函数
long allocateMemory(long bytes);
// 扩充内存
long reallocateMemory(long address, long bytes);
// 释放内存
void freeMemory(long address);
// 在给定的内存块中设置值
void setMemory(Object o, long offset, long bytes, byte value);
// 内存拷贝
void copyMemory(Object srcBase, long srcOffset, Object destBase, long destOffset, long bytes);
//  获取给定地址值，忽略修饰限定符的访问限制。与此类似操作还有: getInt，getDouble，getLong，getChar 等
Object  getObject(Object o, long offset);
// 为给定地址设置值，忽略修饰限定符的访问限制，与此类似操作还有:putInt, putDouble，putLong，putChar 等
void putObject(Object o, long offset, Object x);
// 获取给定地址的 byte 类型的值（当且仅当该内存地址为 allocateMemory 分配时，此方法结果为确定的）
byte getByte(long address);
// 为给定地址设置 byte 类型的值（当且仅当该内存地址为 allocateMemory 分配时，此方法结果才是确定的）
void putByte(long address, byte x);
```

通常，我们在 Java 中创建的对象都处于堆内内存（heap）中，堆内内存是由
JVM 所管控的 Java 进程内存，并且它们遵循 JVM 的内存管理机制，JVM 会采用
垃圾回收机制统一管理堆内存。与之相对的是堆外内存，存在于 JVM 管控之外的内
存区域，Java 中对堆外内存的操作，依赖于 Unsafe 提供的操作堆外内存的 native
方法。

**使用堆外内存的原因**

- 对垃圾回收停顿的改善。由于堆外内存是直接受操作系统管理而不是 JVM，所以当我们使用堆外内存时，即保持较小的堆内内存规模。从而在 GC 时减少回收停顿对于应用的影响。
- 提升程序 I/O 操作的性能。通常在 I/O 通信过程中，会存在堆内内存到堆外内存的数据拷贝操作，对于需频繁进行内存间数据拷贝且生命周期较短的暂存数据，都建议存储到堆外内存。

**典型应用**

DirectByteBuffer 是 Java 用于实现堆外内存的一个重要类，通常用在通信过程中做缓冲池，如在 Netty、MINA 等 NIO 框架中应用广泛。DirectByteBuffer 对于堆外内存的创建、使用、销毁等逻辑均由 Unsafe 提供的堆外内存 API 来实现。

## CAS

什么是 CAS? 即比较并替换，实现并发算法时常用到的一种技术。CAS 操作包含三个操作数——内存位置、预期值及新值。执行 CAS 操作的时候，将内存位置的值与预期原值比较，如果相匹配，那么处理器会自动将该位置值更新为新值，否则，处理器不做任何操作。我们都知道，CAS 是一条 CPU 的原子指令（cmpxchg 指令），不会造成所谓的数据不一致问题，Unsafe 提供的 CAS 方法（如compareAndSwapXXX）底层实现即为 CPU 指令 cmpxchg。

```java
boolean compareAndSwapObject(Object o, long offset, Object expected, Object update);
boolean compareAndSwapInt(Object o, long offset, int expected,int update);
boolean compareAndSwapLong(Object o, long offset, long expected, long update);
```

**典型应用**

CAS 在 java.util.concurrent.atomic 相关类、Java AQS、CurrentHashMap 等实现上有非常广泛的应用。AtomicInteger 的实现中，静态字段 valueOffset 即为字段 value 的内存偏移地址，valueOffset 的值在 AtomicInteger 初 始 化 时， 在 静 态 代 码 块 中 通 过 Unsafe 的 objectFieldOffset 方 法 获 取。 在 AtomicInteger 中提供的线程安全方法中，通过字段 valueOffset 的值可以定位到 AtomicInteger 对象中 value 的内存地址，从而可以根据 CAS 实现对 value 字段的原子操作。

## 线程调度

这部分，包括线程挂起、恢复、锁机制等方法。

```java
// 取消阻塞线程
void unpark(Object thread);
// 阻塞线程
void park(boolean isAbsolute, long time);
// 获得对象锁（可重入锁）
@Deprecated
void monitorEnter(Object o);
// 释放对象锁
@Deprecated
void monitorExit(Object o);
// 尝试获取对象锁
@Deprecated
boolean tryMonitorEnter(Object o);
```

如上源码说明中，方法 park、unpark 即可实现线程的挂起与恢复，将一个线程进行挂起是通过 park 方法实的，调用 park 方法后，线程将一直阻塞直到超时或者中断等条件出现；unpark 可以终止一个挂起的线程，使其恢复正常。

**典型应用**

Java 锁和同步器框架的核心类 AbstractQueuedSynchronizer，就是通过调用 LockSupport.park() 和 LockSupport.unpark() 实现线程的阻塞和唤醒的，而 LockSupport 的 park、unpark 方法实际是调用 Unsafe 的 park、unpark 方式来实现。

## Class 相关

此部分主要提供 Class 和它的静态字段的操作相关方法，包含静态字段内存定位、定义类、定义匿名类、检验 & 确保初始化等。

```java
// 获取给定静态字段的内存地址偏移量，这个值对于给定的字段是唯一且固定不变的
long staticFieldOffset(Field f);
// 获取一个静态类中给定字段的对象指针
Object staticFieldBase(Field f);
// 判断是否需要初始化一个类，通常在获取一个类的静态属性的时候（因为一个类如果没初始化，它的静态属性也不会初始化）使用。 当且仅当 ensureClassInitialized 方法不生效时返回 false。
boolean shouldBeInitialized(Class<?> c);
// 检测给定的类是否已经初始化。通常在获取一个类的静态属性的时候（因为一个类如果没初始化，它的静态属性也不会初始化）使用。
void ensureClassInitialized(Class<?> c);
// 定义一个类，此方法会跳过 JVM 的所有安全检查，默认情况下，ClassLoader（类加载器）和 ProtectionDomain（保护域）实例来源于调用者
Class<?> defineClass(String name, byte [] b, int off, int len, ClassLoader loader,ProtectionDomain protectionDomain);
// 定义一个匿名类
Class<?> defineAnonymousClass(Class<?> hostClass, byte[] data, Object[] cpPatches);
```

**典型应用**

从 Java 8 开始，JDK 使用 invokedynamic 及 VM Anonymous Class 结合来实现 Java 语言层面上的 Lambda 表达式。

- invokedynamic： invokedynamic 是 Java 7 为了实现在 JVM 上运行动态语言而引入的一条新的虚拟机指令，它可以实现在运行期动态解析出调用点限定符所引用的方法，然后再执行该方法，invokedynamic 指令的分派逻辑是由用户设定的引导方法决定。
- VM Anonymous Class：可以看做是一种模板机制，针对于程序动态生成很多结构相同、仅若干常量不同的类时，可以先创建包含常量占位符的模板类，而后通过 Unsafe.defineAnonymousClass 方法定义具体类时填充模板的占位符生成具体的匿名类。生成的匿名类不显式挂在任何 ClassLoader 下面，只要当该类没有存在的实例对象、且没有强引用来引用该类的 Class 对象时，该类就会被 GC 回收。故而 VM Anonymous Class 相比于 Java 语言层面的匿名内部类无需通过 ClassClassLoader 进行类加载且更易回收。

在 Lambda 表达式实现中，通过 invokedynamic 指令调用引导方法生成调用点，在此过程中，会通过 ASM 动态生成字节码，而后利用 Unsafe 的 defin-eAnonymousClass 方法定义实现相应的函数式接口的匿名类，然后再实例化此匿名类，并返回与此匿名类中函数式方法的方法句柄关联的调用点；而后可以通过此调用点实现调用相应 Lambda 表达式定义逻辑的功能。

## 对象操作

此部分主要包含对象成员属性相关操作及非常规的对象实例化方式等相关方法。

```java
// 返回对象成员属性在内存地址相对于此对象的内存地址的偏移量
long objectFieldOffset(Field f);
// 获得给定对象的指定地址偏移量的值，与此类似操作还有：getInt，getDouble，getLong，getChar 等
Object getObject(Object o, long offset);
// 给定对象的指定地址偏移量设值，与此类似操作还有：putInt，putDouble，putLong，putChar 等
void putObject(Object o, long offset, Object x);
// 从对象的指定偏移量处获取变量的引用，使用 volatile 的加载语义
Object  getObjectVolatile(Object o, long offset);
// 存储变量的引用到对象的指定的偏移量处，使用 volatile 的存储语义
void putObjectVolatile(Object o, long offset, Object x);
// 有序、延迟版本的 putObjectVolatile 方法，不保证值的改变被其他线程立即看到。只有在 field 被 volatile 修饰符修饰时有效
void putOrderedObject(Object o, long offset, Object x);
// 绕过构造方法、初始化代码来创建对象
Object allocateInstance(Class<?> cls) throws InstantiationException;
```

**典型应用**

- 常规对象实例化方式：我们通常所用到的创建对象的方式，从本质上来讲，都是通过 new 机制来实现对象的创建。但是，new 机制有个特点就是当类只提供有参的构造函数且无显示声明无参构造函数时，则必须使用有参构造函数进行对象构造，而使用有参构造函数时，必须传递相应个数的参数才能完成对象实例化。
- 非常规的实例化方式：而 Unsafe 中提供 allocateInstance 方法，仅通过 Class 对象就可以创建此类的实例对象，而且不需要调用其构造函数、初始化代码、JVM 安全检查等。它抑制修饰符检测，也就是即使构造器是 private 修饰的也能通过此方法实例化，只需提类对象即可创建相应的对象。由于这种特性，allocateInstance 在 java.lang.invoke、Objenesis（提供绕过类构造器的对象生成方式）、Gson（反序列化时用到）中都有相应的应用。

## 数组相关

这部分主要介绍与数据操作相关的 arrayBaseOffset 与 arrayIndexScale 这两个方法，两者配合起来使用，即可定位数组中每个元素在内存中的位置。

```java
// 返回数组中第一个元素的偏移地址
int arrayBaseOffset(Class<?> arrayClass);
// 返回数组中一个元素占用的大小
int arrayIndexScale(Class<?> arrayClass);
```

**典型应用**

这两个与数据操作相关的方法，在 java.util.concurrent.atomic 包下的 Ato-micIntegerArray（可以实现对 Integer 数组中每个元素的原子性操作）中有典型的应用，如下图 AtomicIntegerArray 源码所示，通过 Unsafe 的 arrayBaseOffset、arrayIndexScale 分别获取数组首元素的偏移地址 base 及单个元素大小因子 scale。后续相关原子性操作，均依赖于这两个值进行数组中元素的定位，如下图二所示的getAndAdd 方法即通过 checkedByteOffset 方法获取某数组元素的偏移地址，而后通过 CAS 实现原子性操作。

## 内存屏障

在 Java 8 中引入，用于定义内存屏障（也称内存栅栏，内存栅障，屏障指令等，是一类同步屏障指令，是 CPU 或编译器在对内存随机访问的操作中的一个同步点，使得此点之前的所有读写操作都执行后才可以开始执行此点之后的操作），避免代码重排序。

```java
// 内存屏障，禁止 load 操作重排序。屏障前的 load 操作不能被重排序到屏障后，屏障后的 load 操作不能被重排序到屏障前
void loadFence() ;
// 内存屏障，禁止 store 操作重排序。屏障前的 store 操作不能被重排序到屏障后，屏障后的 store 操作不能被重排序到屏障前
void storeFence() ;
// 内存屏障，禁止 load、store 操作重排序
void fullFence() ;
```

**典型应用**

在 Java 8 中引入了一种锁的新机制——StampedLock，它可以看成是读写锁的一个改进版本。StampedLock 提供了一种乐观读锁的实现，这种乐观读锁类似于无锁的操作，完全不会阻塞写线程获取写锁，从而缓解读多写少时写线程“饥饿”现象。由于 StampedLock 提供的乐观读锁不阻塞写线程获取读锁，当线程共享变量从主内存 load 到线程工作内存时，会存在数据不一致问题，所以当使用 StampedLock 的乐观读锁时，需要遵从如下图用例中使用的模式来确保数据的一致性。

## 系统相关

这部分包含两个获取系统相关信息的方法。

```java
// 返回系统指针的大小。返回值为 4（32 位系统）或 8（64 位系统）。
int addressSize() ;
// 内存页的大小，此值为 2 的幂次方。
int pageSize() ;
```

**典型应用**

java.nio 下的工具类 Bits 中计算待申请内存所需内存页数量的静态方法，其依赖于 Unsafe 中 pageSize 方法获取系统内存页大小实现后续计算逻辑。

# FFI

[Project Panama](https://openjdk.java.net/projects/panama/)<br>
