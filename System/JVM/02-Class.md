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
- [操作码](#操作码)
    - [构造方法](#构造方法)
        - [`<cinit>()V`](#cinitv)
        - [`<init>()V`](#initv)
    - [多态原理](#多态原理)
        - [invokevirtual](#invokevirtual)

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

```c
ClassFile {
    u4             magic;
    u2             minor_version;
    u2             major_version;
    u2             constant_pool_count;
    cp_info        constant_pool[constant_pool_count - 1];
    u2             access_flags;
    u2             this_class;
    u2             super_class;
    u2             interfaces_count;
    u2             interfaces[interfaces_count];
    u2             fields_count;
    field_info     fields[fields_count];
    u2             methods_count;
    method_info    methods[methods_count];
    u2             attributes_count;
    attribute_info attributes[attributes_count];
}
```

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

# 操作码

## 构造方法

### `<cinit>()V`

`<cinit>()V` 方法会在类加载的初始化阶段被调用.

```java
public class Cinit {
    static int i = 10;

    static {
        i = 20;
    }

    static {
        i = 30;
    }
}
```

编译器会按从上至下的顺序, 收集所有 `static` 静态代码块和静态成员赋值的代码, 合并为一个特殊的方法 `<cinit>()V`:

```sh
 0: bipush    10
 2: putstatic #2 // Field i:I
 5: bipush    20
 7: putstatic #2 // Field i:I
10: bipush    30
12: putstatic #2 // Field i:I
15: return
```

### `<init>()V`

```java
public class Init {
    private String a = "s1";

    {
        b = 20;
    }

    private int b = 10;

    {
        a = "s2";
    }

    public Init(String a, int b) {
        this.a = a;
        this.b = b;
    }

    public static void main(String[] args) {
        Init i = new Init("s3", 30);
        System.out.println(i.a);
        System.out.println(i.b);
    }
}
```

编译器会按从上至下的顺序, 收集所有 `{}` 代码块和成员变量赋值的代码, 形成新的构造方法, 但原始构造方法内的代码总是在最后

## 多态原理

1. 运行代码
2. 运行 HSDB(sun.jvm.hotspot.HSDB) 工具
3. 查找类的 vtable

### invokevirtual

1. 先通过栈帧中的对象引用找到对象
2. 分析对象头, 找到对象的实际 class
3. class 结构中有 vtable, 它在类加载的链接阶段就已经根据方法的重写规则生成好了
4. 查表得到方法的具体地址
5. 执行方法的字节码
