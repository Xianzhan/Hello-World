<!-- TOC -->

- [历史](#历史)
    - [C90 标准](#c90-标准)
    - [C99 标准](#c99-标准)
    - [C11 标准](#c11-标准)
- [资源](#资源)
    - [书籍](#书籍)

<!-- /TOC -->

# 历史

> C 语言最初由 Dennis Ritchie 于 1969 年到 1973 年在 AT&T 贝尔实验室里开发出来，主要用于重新实现 Unix 操作系统。此时，C 语言又被称为 K&R C。其中，K 表示 Kernighan 的首字母，而 R 则是 Ritchie 的首字母。K&R C 语言与后来标准化的 C 语言有很大差异。比如，如果函数返回类型为 int，则 int 可省：int my_function() { }，也可以写成 my_function(){ }。编译器不会有任何警告，更不会报错。另外，还有现在看来比较奇葩的函数定义，像我们现在定义这么一个函数—— void my_function(int a, char*p){ }，如果是用 K&R C 语法定义的话要写成：void my_function(a, p) int a; char *p; { }。K&R 的 C 语法中，定义一个函数时，其形参列表先列出形参的标识符，然后在函数声明的后面紧跟着对形参标识符的完整声明，最后是函数体。这在现行标准中已经被逐步废弃使用了。另外，当时的第一本 C 语言专业书《The C ProgrammingLanguage》也并非一个正式的编程语言规范，但被用了许多年。

> C 语言的发明其实基于 Unix 操作系统。当时在 C 语言未面世之前，Dennis Ritchie 所在的 AT&T 贝尔实验室用的 Unix 系统是完全用汇编语言写的。汇编语言的优势是直接面向处理器本身，能直接对底层硬件进行控制，充分发挥处理器的硬件能力。然而，它的缺陷也是显而易见的。

> 汇编语言的不足: 首先，不可移植性。每种处理器，其指令集都大相径庭，比如 ARM 有 ARM 的指令集架构（ISA），Intel x86 有 x86 的 ISA，还有 MIPS、Power（原来为 PowerPC），Motorola 68000 等；再加上各类微控制器单元（Micro-Controller Unit, MCU）、各类数字信号处理器（DigitalSignal Processor, DSP），每种 ISA 都有其相应的汇编语言。那么多处理器如果对每一种都使用不同的汇编语言来实现同一个操作系统，那操作系统的开发人员真要崩溃了……而且即便实现出来，可能各个处理器上的实现也会有所不同，标准也很难被统一起来。其次，汇编语言本身要比高级语言精密。因为汇编语言面对的都是寄存器、存储器以及各类底层硬件，而不是一种抽象的数据模型，所以代码编写时需要非常谨慎，而且调试程序也十分麻烦，且非常容易出错。所以，如果有一种既能面向底层硬件，又能对数据以及程序进行抽象的高级语言出现，那势必既能不太影响程序执行效率，又能大大提升程序的可执行性、可读性以及编写的效率，这将是非常伟大的贡献。C 语言也就是在这种背景下诞生的。

## C90 标准

> 由于 C 语言被各大公司所使用（包括当时处于鼎盛时期的 IBM PC），因此到了 1989 年，C 语言由美国国家标准协会（ANSI）进行了标准化，此时 C 语言又被称为 ANSI C。而仅过一年，ANSI C 就被国际标准化组织 ISO 给采纳了。此时，C 语言在 ISO 中有了一个官方名称—— ISO/IEC9899:1990。其中，9899 是 C 语言在 ISO 标准中的代号，像 C++ 在 ISO 标准中的代号是 14882。而冒号后面的 1990 表示当前修订好的版本是在 1990 年发布的。对于 ISO/IEC9899:1990 的俗称或简称，有些地方称为 C89，有些地方称为 C90，或者 C89/90。不管怎么称呼，它们都指代这个最初的 C 语言国际标准。这个版本的 C 语言标准作为 K&R C 的一个超集（即 K&R C 是此标准 C 的一个子集），把后来引入的许多非官方特性也一起整合了进去。其中包括了从 C++ 借鉴的函数原型（Function Prototypes），指向 void 的指针，对国际字符集以及本地语言环境的支持。在此标准中，尽管已经将函数定义的方式改为现在我们常用的那种方式，不过 K&R 的语法形式仍然兼容。

## C99 标准

> 在随后的几年里，C 语言的标准化委员会又不断地对 C 语言进行改进，到了 1999 年，正式发布了 ISO/IEC9899:1999，简称为 C99 标准。C99 标准引入了许多特性，包括内联函数（inline functions）、可变长度的数组、灵活的数组成员（用于结构体）、复合字面量、指定成员的初始化器、对 IEEE754 浮点数的改进、支持不定参数个数的宏定义，在数据类型上还增加了 long long int 以及复数类型。毫不夸张地说，即便到目前为止，很少有 C 语言编译器是完整支持 C99 的。像主流的 GCC 以及 Clang 编译器都能支持高达 90% 以上，而微软的 Visual Studio 2015 中的 C 编译器只能支持到 70% 左右。关于 C 语言历史与演化进程的详细介绍可参考维基百科：https://en.wikipedia.org/wiki/C_(programming_language)。C 语言标准委员会的最新动态（可参见：http://www.open-std.org/jtc1/sc22/wg14/），其中看到有人提出想为 C 语言添加面向对象的特性，包括增加类、继承、多态等已被 C++ 语言所广泛使用的语法特性，但是最终被委员会驳回了。因为这些复杂的语法特性并不符合 C 语言的设计理念以及设计哲学，况且 C++ 已经有了这些特性，C 语言无需再对它们进行支持。

## C11 标准

> 2007 年，C 语言标准委员会又重新开始修订 C 语言，到了 2011 年正式发布了 ISO/IEC 9899:2011，简称为 C11 标准。C11 标准新引入的特征尽管没 C99 相对 C90 引入的那么多，但是这些也都十分有用，比如：字节对齐说明符、泛型机制（generic selection）、对多线程的支持、静态断言、原子操作以及对 Unicode 的支持。

# 资源

[Github://MicrosoftDocs/cpp-docs](https://github.com/MicrosoftDocs/cpp-docs)<br>
[Github://luguanxing/Cheating-Plugin-Program](https://github.com/luguanxing/Cheating-Plugin-Program)<br>
[Github://jiejieTop/TencentOS-Demo](https://github.com/jiejieTop/TencentOS-Demo)<br>
[cppreference.com](https://en.cppreference.com/w/)<br>
[【C++】预编译、编译、汇编、链接](https://blog.csdn.net/weixin_40740059/article/details/84075653)<br>
[C语言：春节回家过年，我发现只有我没有对象！](https://mp.weixin.qq.com/s/QT6abxt5evqxU8kDuhfdHQ)<br>
[C++ 内存模型](https://paul.pub/cpp-memory-model/)<br>

## 书籍

《C 语言编程魔法书: 基于 C11 标准》<br>
