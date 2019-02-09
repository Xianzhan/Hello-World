<!-- TOC -->

- [内存结构](#内存结构)
    - [线程隔离](#线程隔离)
        - [虚拟机栈(stack)](#虚拟机栈stack)
        - [本地方法栈(native stack)](#本地方法栈native-stack)
        - [程序计数器(program counter register)](#程序计数器program-counter-register)
    - [线程共享](#线程共享)
        - [元空间](#元空间)
        - [堆(heap)](#堆heap)
- [class 文件格式](#class-文件格式)
    - [类加载](#类加载)
        - [加载 Loading](#加载-loading)
        - [链接 Linking](#链接-linking)
        - [初始化 Initalization](#初始化-initalization)
        - [使用 Using](#使用-using)
        - [卸载 Unloading](#卸载-unloading)
    - [字节码](#字节码)
- [GC](#gc)
    - [垃圾回收算法](#垃圾回收算法)
        - [引用计数法](#引用计数法)
        - [根搜索算法](#根搜索算法)
    - [垃圾回收器](#垃圾回收器)
        - [Serial](#serial)
        - [Parallel Scavenge](#parallel-scavenge)
        - [Parallel New](#parallel-new)
        - [Serial Old](#serial-old)
        - [Parallel Old](#parallel-old)
        - [CMS](#cms)
        - [G1](#g1)

<!-- /TOC -->

Java 虚拟机（Java Virtual Machine）

# 内存结构

![https://static001.geekbang.org/resource/image/ab/77/ab5c3523af08e0bf2f689c1d6033ef77.png](../.resource/System-JVM-memory_model.png)

## 线程隔离

### 虚拟机栈(stack)

是线程私有的，声明周期与线程相同，虚拟机栈是 Java 方法执行的内存模型，每个方法被执行时都会创建一个栈帧，即方法运行期间的基础数据结构，栈帧用于存储：局部变量表、操作数栈、动态链接、方法出口等，每个方法执行中都对应虚拟机栈帧从入栈到处栈的过程。

是一种数据结构，是虚拟机中的局部变量表，对应物理层之上的程序数据模型。

局部变量表，是一种程序运行数据模型，存放了编译期可知的各种数据类型例如：

boolean、byte、char、short、int、float、long、double、对象引用类型(对象内存地址变量，指针或句柄)，程序运行时，根据局部变量表分配栈帧空间大小，在运行中，大小是不变的异常类型：StackOverFlowError 线程请求栈深度大于虚拟机允许深度 OutOfMemory 内存空间耗尽无法进行扩展。

### 本地方法栈(native stack)

与虚拟机栈类似，虚拟机栈为 Java 程序服务，本地方法栈支持虚拟机的运行服务，具体实现由虚拟机厂商决定，也会抛出 StackOverFlowError、OutOfMemory 异常。

### 程序计数器(program counter register)

行号指示器，字节码指令的分支、循环、跳转、异常处理、线程恢复(CPU 切换)，每条线程都需要一个独立的计数器，线程私有内存互不影响,该区域不会发生内存溢出异常。

## 线程共享

Java 1.7 之前分为 `堆`, `永久代`(`方法区`, `常量区`)<br>
Java 1.7 分为 `堆`, `永久代`(`方法区`)<br>
Java 8 分为 `堆`, `元空间`<br>

### 元空间

Java 8 中用元空间替代了永久代, 元空间并不在虚拟机中，而是使用本地内存

**方法区(method area)**

与堆一样属于线程共享的内存区域，用于存储虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码（动态加载OSGI）等数据。理论上属于 java 虚拟机的一部分，为了区分开来叫做 Non-Heap 非堆。

这个区域可以选择不进行垃圾回收，该区域回收目的主要是常量池的回收，及类型的卸载class,内存区不足时会抛出 OutOfMemory 异常

**运行时常量池**

方法区的一部分，Class 的版本、字段、接口、方法等，及编译期生成的各种字面量、符号引用，编译类加载后存放在该区域。会抛出 OutOfMemory 异常。

### 堆(heap)

![https://time.geekbang.org/column/article/13137](../.resource/System-JVM-memory_heap.png)

是虚拟机管理内存中最大的一部分，被所有线程共享，用于存放对象实例(对象、数组)，物理上不连续的内存空间，由于 GC 收集器，分代收集，所以划分为：新生代 Eden、From SurVivor 空间、To SurVivor 空间，allow buffer(分配空间)，可能会划分出多个线程私有的缓冲区，老年代。

**年轻代(Young)**

年轻代一般占堆的 1/3, 大多数对象在新生代中被创建，其中很多对象的生命周期很短.

年轻代内又分三个区：一个 Eden(8/10) 区，两个 Survivor(from 1/10, to 1/10) 区，大部分对象在 Eden 区中生成。每次使用 Eden 和其中一块 Survivor。当回收时，将 Eden 和 Survivor 中还存活的对象一次性地拷贝到另外一块 Survivor 空间上，最后清理掉 Eden 和刚才用过的 Survivor.

**老年代(Old)**

老年代（Old Generation）一般占堆的 2/3, 在新生代中经历了 N 次垃圾回收后仍然存活的对象，就会被放到年老代，或者大对象直接进入老年代。该区域中对象存活率高。

# class 文件格式

1. 基本信息，涵盖了原 class 文件的相关信息。
2. 常量池，用来存放各种常量以及符号引用。
3. 字段区域，用来列举该类中的各个字段。
4. 方法区域，用来列举该类中的各个方法。

Java 虚拟机所使用的一种平台中立(不依赖于特定硬件及操作系统)的二进制格式表示。

[一张图看懂JVM之类装载系统](https://mp.weixin.qq.com/s/UU4qltVgRsj0SG7YmER-Qw)<br>

## 类加载

### 加载 Loading

加载是由 ClassLoader 来完成的，ClassLoader 的主要作用是将 Java 字节码转换成 JVM 中的 java.lang.Class 类对象，其次通过双亲委派模式保证了类加载的唯一性和安全性；但是由于双亲委派的存在，所以启动一个类加载过程的类加载器和最终定义这个类的类加载器可能并不是一个（前者称为初始类加载器，后者称为定义类加载器），故一个 Java 类的定义类加载器是该类所导入的其它 Java 类的初始类加载器（譬如 A、B 两个类均未加载，A 中定义了 B 类型的成员，则 A 的定义类加载器会负责启动 B 的加载过程），这个一定要理解。**此外 JVM 判断两个 java.lang.Class 是否相同不仅要依据类的全路径描述符，还要保证是同一个 ClassLoader 加载。**

### 链接 Linking

链接是将 Java 类的二进制代码合并到 JVM 运行状态中的过程，链接依赖于成功的加载流程，链接包括验证、准备和解析等几个步骤。

**验证 Verification**

验证过程是确保 Java 类二进制结构字节码的正确性，如果验证过程出现错误则会抛出 java.lang.VerifyError 错误。

**准备 Preparation**

准备过程将创建 Java 类的静态域，同时将这些域的值设为默认值，准备过程不会执行代码（不会执行，不会执行，重要的事情说三遍）。

**解析 Resolution**

解析过程确保类的继承、组合等关联类、接口能被正确找到，解析的过程可能会导致其它的 Java 类被加载（譬如在一个 Java 类中会包含对其它类或接口的引用或继承，解析就是去确认这些被引用的类能被正确的找到）。不同 JVM 解析策略可能不同，有些是在链接的时候就递归对所有依赖进行解析，有些只会在真正用到时时才进行解析（譬如只在方法中使用到了其他类）。

### 初始化 Initalization

初始化是指一个类第一次被使用时 JVM 会进行该类的初始化操作，初始化过程主要是执行类里面的静态代码块和初始化静态域。在一个类被初始化之前，它的直接父类也需要被初始化。但是一个接口的初始化不会引起其父接口的初始化。在初始化的时候，会按照源代码中从上到下的顺序依次执行静态代码块或初始化静态域。

Java 类和接口的初始化过程只会在特定时机发生，具体如下：

- 创建一个未被使用过的 Java 类实例。
- 调用一个未被使用过的 Java 类静态方法。
- 给一个未被使用过的 Java 类或接口中声明的静态域赋值。
- 访问一个未被使用过的 Java 类或接口中声明的静态域且该域不是常值变量。

### 使用 Using

### 卸载 Unloading

## 字节码

1. `invokestatic`: 用于调用静态方法。
2. `invokespecial`: 用于调用私有实例方法、构造器，以及使用 `super` 关键字调用父类的实例方法或构造器，和所实现接口的默认方法。
3. `invokevirtual`: 用于调用非私有实例方法。
4. `invokeinterface`: 用于调用接口方法。
5. `invokedynamic`: 用于调用动态方法。

# GC

## 垃圾回收算法

### 引用计数法

### 根搜索算法

## 垃圾回收器

### Serial

年轻代，单线程，标记-复制算法。

### Parallel Scavenge

年轻代，多线程，标记-复制算法。不能与 [CMS](#CMS) 一起使用。

### Parallel New

年轻代，多线程，标记-复制算法，更加注重吞吐率。

### Serial Old

老年代，单线程，标记-压缩算法。

### Parallel Old

老年代，多线程，标记-压缩算法。

### CMS

老年代，并发的，标记-清除算法。可在程序运行过程中进行垃圾回收，在并发收集失败的情况下，Java 虚拟机会使用其他两个压缩型垃圾回收器进行一次垃圾回收。由于 G1 的出现，CMS 在 Java 9 中被废弃。

### G1

G1 (Garbage First) 是一个横跨新生代和老年代的垃圾回收器。直接将堆分成极其多个区域，每个区域都可以充当 Eden 区、Survivor 区或者老年代中的一个。采用 标记-压缩算法，而且和 CMS 一样都能够在应用程序运行过程并发地进行垃圾回收。优先回收死亡对象较多的区域。

[好文推荐：CMS学习笔记！！！](https://mp.weixin.qq.com/s/-yqJa4dOyzLaK_tJ1x9E7w)<br>
[好文推荐：G1学习笔记！！！](https://mp.weixin.qq.com/s/CG-k-Vqw3LVUyUjnDpPmFw)<br>