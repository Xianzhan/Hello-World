<!-- TOC -->

- [字节码](#字节码)
    - [魔数](#魔数)
    - [版本号](#版本号)
    - [常量池](#常量池)
    - [访问标志](#访问标志)
    - [当前类名](#当前类名)
    - [父类名称](#父类名称)
    - [接口信息](#接口信息)
    - [字段表](#字段表)
    - [方法表](#方法表)
    - [附加属性表](#附加属性表)
- [生命周期](#生命周期)
    - [加载 Loading](#加载-loading)
    - [链接 Linking](#链接-linking)
    - [初始化 Initalization](#初始化-initalization)
    - [使用 Using](#使用-using)
    - [卸载 Unloading](#卸载-unloading)

<!-- /TOC -->

# 字节码

> Java 之所以可以“一次编译，到处运行”，一是因为 JVM 针对各种操作系统、平台都进行了定制，二是因为无论在什么平台，都可以编译生成固定格式的字节码（.class 文件）供 JVM 使用。因此，也可以看出字节码对于 Java 生态的重要性。之所以被称之为字节码，是因为字节码文件由十六进制值组成，而 JVM 以两个十六进制值为一组，即以字节为单位进行读取。在 Java 中一般是用 `javac` 命令编译源代码为字节码文件，一个 .java 文件从编译到运行的示例如图:

```sh
    .java file
         ↓
 Java Compiler(javac)
         ↓
    .class file
         ↓
    ClassLoader ------+
         ↓            |
  BytecodeVerifier    |
         ↓            +- Java Virtual Machine
 Java Runtime System  |
         ↓            |
      Native OS ------+
```

编译后的 class 文件需要符合 JVM 规范, 每一个字节码文件都要由十部分按照固定的顺序组成:

1. 魔数(Magic)
2. 版本号(Version)
3. 常量池(constant_pool)
4. 访问标志(access_flag)
5. 当前类索引(this_class)
6. 父类索引(super_class)
7. 接口索引(interfaces)
8. 字段表(fields)
9. 方发表(methods)
10. 附加属性(attributes)

## 魔数

所有的 .class 文件的前四个字节都是魔数，魔数的固定值为：`0xCAFEBABE`。魔数放在文件开头，JVM 可以根据文件的开头来判断这个文件是否可能是一个 .class 文件，如果是，才会继续进行之后的操作。

## 版本号

版本号为魔数之后的 4 个字节，前两个字节表示次版本号（Minor Version），后
两个字节表示主版本号（Major Version）。

## 常量池

紧接着主版本号之后的字节为常量池入口。常量池中存储两类常量：字面量与符号引用。字面量为代码中声明为 Final 的常量值，符号引用如类和接口的全局限定名、字段的名称和描述符、方法的名称和描述符。常量池整体上分为两部分：常量池计数器以及常量池数据区。

- 常量池计数器（constant_pool_count）：由于常量的数量不固定，所以需要先放置两个字节来表示常量池容量计数值。
- 常量池数据区：数据区是由（constant_pool_count-1）个 cp_info 结构组成，一个 cp_info 结构对应一个常量。在字节码中共有 14 种类型的 cp_info，每种类型的结构都是固定的。

<table>
    <tr>
        <th>常量</th><th>项目</th><th>类型</th><th>描述</th>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_Utf8_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 1</td>
    </tr>
    <tr>
        <td>length</td>
        <td>2byte</td>
        <td>UTF-8 编码字符串占用的字符数</td>
    </tr>
    <tr>
        <td>bytes</td>
        <td>1byte</td>
        <td>长度为 length 的 UTF-8 编码的字符串</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_Integer_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 3</td>
    </tr>
    <tr>
        <td>bytes</td>
        <td>4byte</td>
        <td>按照高位在前存储的 int 值</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_Float_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 4</td>
    </tr>
    <tr>
        <td>bytes</td>
        <td>4byte</td>
        <td>按照高位在前存储的 float 值</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_Long_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 5</td>
    </tr>
    <tr>
        <td>bytes</td>
        <td>8byte</td>
        <td>按照高位在前存储的 long 值</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_Double_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 6</td>
    </tr>
    <tr>
        <td>bytes</td>
        <td>8byte</td>
        <td>按照高位在前存储的 double 值</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_Class_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 7</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向全限定名常量项的索引</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_String_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 8</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向字符串字面量的索引</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_Fieldref_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 9</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向声明字段的类或者接口描述符 CONSTANT_Class_info 的索引项</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向字段描述符 CONSTANT_NameAndType 的索引项</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_Methodref_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 10</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向声明方法的类描述符 CONSTANT_Class_info 的索引项</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向名称及类型描述符 CONSTANT_NameAndType 的索引项</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_InterfaceMethodref_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 11</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向声明方法的接口描述符 CONSTANT_Class_info 的索引项</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向名称及类型描述符 CONSTANT_NameAndType 的索引项</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_NameAndType_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 12</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向该字段或方法名称常量项的索引项</td>
    </tr>
    <tr>
        <td>index</td>
        <td>2byte</td>
        <td>指向该字段或方法描述符常量项的索引项</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_MethodHandle_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 15</td>
    </tr>
    <tr>
        <td>reference_kind</td>
        <td>1byte</td>
        <td>值必须在 1~9 之间, 它决定了方法句柄的类型方法, 句柄类型的值表示方法句柄的字节码行为</td>
    </tr>
    <tr>
        <td>reference_index</td>
        <td>2byte</td>
        <td>值必须是对常量池的有效索引</td>
    </tr>
    <tr>
        <td rowspan="2">CONSTANT_MethodType_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 16</td>
    </tr>
    <tr>
        <td>descriptor_index</td>
        <td>2byte</td>
        <td>值必须是对常量池的有效索引, 常量池在该索引处的项必须是 CONSTANT_Utf8_info 结构, 表示方法的描述符</td>
    </tr>
    <tr>
        <td rowspan="3">CONSTANT_InvokeDynamic_info</td>
        <td>tag</td>
        <td>1byte</td>
        <td>值为 18</td>
    </tr>
    <tr>
        <td>bootstrap_method_attr_index</td>
        <td>2byte</td>
        <td>值必须是对当前 Class 文件中引导方法表的 bootstrap_methods[] 数组的有效索引</td>
    </tr>
    <tr>
        <td>name_and_type_index</td>
        <td>2byte</td>
        <td>值必须是对当前常量池的有效索引, 常量池在该索引处的项必须是 CONSTANT_NameAndType_info 结构, 表示方法名和方法描述符</td>
    </tr>
</table>

## 访问标志

常量池结束之后的两个字节，描述该 Class 是类还是接口，以及是否被 Public、
Abstract、Final 等修饰符修饰。需要注意的是，JVM 并没有穷举所有的访问标志，而是使用按位或操作来进
行描述的，比如某个类的修饰符为 Public Final，则对应的访问修饰符的值为 ACC_
PUBLIC | ACC_FINAL，即 0x0001 | 0x0010=0x0011。

标志名称|标志值|含义
:---|:---:|:---
ACC_PUBLIC|0x0001|public
ACC_PRIVATE|0x0002|private
ACC_PROTECTED|0x0004|protected
ACC_STATIC|0x0008|static
ACC_FINAL|0x0010|final
ACC_VOLATILE|0x0040|volatile
ACC_TRANSTENT|0x0080|transient
ACC_SYNCHETIC|0x1000|字段是否由编译器自动产生
ACC_ENUM|0x4000|enum

## 当前类名

访问标志后的两个字节，描述的是当前类的全限定名。这两个字节保存的值为常量池中的索引值，根据索引值就能在常量池中找到这个类的全限定名。

## 父类名称

当前类名后的两个字节，描述父类的全限定名，同上，保存的也是常量池中的索引值。

## 接口信息

父类名称后为两字节的接口计数器，描述了该类或父类实现的接口数量。紧接着的 n 个字节是所有接口名称的字符串常量的索引值。

## 字段表

字段表用于描述类和接口中声明的变量，包含类级别的变量以及实例变量，但是不包含方法内部声明的局部变量。字段表也分为两部分，第一部分为两个字节，描述字段个数；第二部分是每个字段的详细信息 fields_info。

## 方法表

字段表结束后为方法表，方法表也是由两部分组成，第一部分为两个字节描述方法的个数；第二部分为每个方的详细信息。方法的详细信息较为复杂，包括方法的访问标志、方法名、方法的描述符以及方法的属性。

## 附加属性表

字节码的最后一部分，该项存放了在该文件中类或接口所定义属性的基本信息。

# 生命周期

```bash
# 类的生命周期

        加载 Loading
            ↓
        链接 Linking
            ↓
      初始化 Initalization
            ↓
        使用 Using
            ↓
        卸载 Unloading
```

**类加载器**

1. 启动类加载器（Bootstrap ClassLoader）：加载 lib/ 中类，不可直接使用
2. 扩展类加载器（Extension ClassLoader）：加载 lib/ext/ 的类库，可直接使用
3. 应用程序类加载器（Application ClassLoader）：负责加载用户类路径上所指定的类，可直接使用。

**双亲委派模型**

如果一个类加载器收到了类加载的请求，他不会自己去尝试加载这个类，而是把请求委派给父类加载器去完成。只有父加载器反馈自己无法完成加载请求时，子加载器才会尝试自己去加载。

## 加载 Loading

类加载指的是类的生命周期中加载、链接、初始化三个阶段.

在加载阶段, 虚拟机需要完成以下 3 件事:

1. 通过一个类的全限定名来获取定义此类的二进制字节流.
2. 将这个字节流代表的静态存储结构转化为方法区的运行时数据结构.
3. 在内存中生成一个代表这个类的 `java.lang.Class` 对象, 作为这个类各种数据的访问入口.

**此外 JVM 判断两个 java.lang.Class 是否相同不仅要依据类的全路径描述符，还要保证是同一个 ClassLoader 加载。**

[一张图看懂JVM之类装载系统](https://mp.weixin.qq.com/s/UU4qltVgRsj0SG7YmER-Qw)<br>

## 链接 Linking

连接阶段比较复杂，一般会跟加载阶段和初始化阶段交叉进行，这个阶段的主要任务就是做一些加载后的验证工作以及一些初始化前的准备工作，可以细分为三个步骤：验证、准备和解析。

**验证 Verification**

当一个类被加载之后，必须要验证一下这个类是否合法，比如这个类是不是符合字节码的格式、变量与方法是不是有重复、数据类型是不是有效、继承与实现是否合乎标准等等。总之，这个阶段的目的就是保证加载的类是能够被 JVM 所运行，并且不会危害虚拟机自身的安全，例如说数组越界访问。

**准备 Preparation**

准备阶段的工作就是为类的静态变量分配内存并设为 JVM 默认的初值，对于非静态的变量，则不会为它们分配内存，这些变量使用的内存都将在方法区中分配。有一点需要注意，这时候，静态变量的初值为 JVM 默认的初值，而不是我们在程序中设定的初值。JVM 默认的初值是这样的：

1. 基本类型(boolean, byte, short, char, int, long, float, double)的默认值为 0.
2. 引用类型的默认值为 `null`
3. 常量的默认值为我们程序中设定的值，比如我们在程序中定义 final static int a = 100，则准备阶段中 a 的初值就是100

**解析 Resolution**

这一阶段的任务就是把常量池中的符号引用转换为直接引用。

- 符号引用：符号引用以一组符号来描述所引用的目标，符号引用可以是任何形式的字面量，只有使用时能无歧义地定位到目标即可。此时目标不一定在内存中。
- 直接引用：直接引用可以是直接指向目标的指针，相对偏移量或者一个能间接定位到目标的句柄。此时目标已经在内存中。

连接阶段完成之后会根据使用的情况（主动引用还是被动引用）来选择是否对类进行初始化。

## 初始化 Initalization

类初始化阶段是类加载过程的最后一步，前面的类加载过程中，除了加载阶段用户应用程序可以通过自定义的类加载器参与之外，其余动作由虚拟机主导和控制。到了本阶段，才真正开始执行类中定义的代码。

初始化过程是执行类构造器 `<clinit>()` 方法的过程。`<clinit>()` 方法是由编译器自动收集类中所有类变量赋值操作和静态语句块中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序决定的。

静态语句块中只能访问到定义在静态语句块之前的变量，定义在它之后的变量，在前面的静态语句块可以赋值，但不能访问。

1. 虚拟机保证父类的 `<clinit>()` 方法在子类的 `<clinit>()` 之前被执行。
2. 虚拟机保证 `<clinit>()` 在多线程环境中被正确地加锁，同步，同一个类加载器下，一个类 `<clinit>()` 方法只会被执行一次。

## 使用 Using

**主动引用**

如果一个类被主动引用，就会触发类的初始化。在 java 中，主动引用的情况有：

1. 通过 `new` 关键字实例化对象、读取或设置类的静态变量、调用类的静态方法。
2. 通过反射方式执行以上三种行为。
3. 初始化子类的时候，会触发父类的初始化。
4. 作为程序入口直接运行时（也就是直接调用 main 方法）。

**被动引用**

1. 引用父类的静态字段，只会引起父类的初始化，而不会引起子类的初始化。
2. 定义类数组，不会引起类的初始化。
3. 引用类的常量，不会引起类的初始化。

## 卸载 Unloading

在类使用完之后，如果满足下面的情况，类就会被卸载：

1. 该类所有的实例都已经被回收，也就是 java 堆中不存在该类的任何实例。
2. 加载该类的 ClassLoader 已经被回收。
3. 该类对应的 `java.lang.Class` 对象没有任何地方被引用，没有在任何地方通过反射访问该类的方法。

如果以上三个条件全部满足，JVM 就会在方法区垃圾回收的时候对类进行卸载，类的卸载过程其实就是在方法区中清空类信息，java 类的整个生命周期就结束了。

由 Java 虚拟机自带的类加载器所加载的类，在虚拟机的生命周期中，始终不会被卸载。Java 虚拟机自带的类加载器包括根类加载器、扩展类加载器和系统类加载器。Java 虚拟机本身会始终引用这些类加载器，而这些类加载器则会始终引用它们所加载的类的 Class 对象，因此这些 Class 对象始终是可触及的。

由用户自定义的类加载器加载的类是可以被卸载的。
