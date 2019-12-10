<!-- TOC -->

- [下载](#下载)
- [编译器](#编译器)
    - [GCC](#gcc)
        - [编译过程](#编译过程)
        - [内部库](#内部库)
        - [外部库](#外部库)
- [基础](#基础)
    - [注释](#注释)
    - [指针](#指针)
    - [宏](#宏)
- [资源](#资源)

<!-- /TOC -->

# 下载

[MinGW-w64 - for 32 and 64 bit Windows](https://sourceforge.net/projects/mingw-w64/files/?source=navbar)<br>

# 编译器

[GCC](http://gcc.gnu.org/)<br>
[LLVM](http://llvm.org/)<br>

## GCC

> 最初的 GNU C 编译器(GCC)是由 GNU 项目的创始人 Richard Stallman 开发的。Richard Stallman 于 1984 年创建了 GNU 项目，以创建一个完整的类 unix 操作系统作为自由软件，以促进计算机用户和程序员之间的自由与合作。<br>
> GCC 的前身是 "GNU C Compiler"，随着时间的推移，它已经支持了许多语言，如 C(gcc)、C++(g++)、Objective-C、Objective-C++、Java(gcj)、Fortran(gfortran)、Ada(gnat)、Go(gccgo)、OpenMP、Cilk Plus 和 OpenAcc。它现在被称为 "GNU Compiler Collection"。

### 编译过程

```shell
            Source Code(.c, .cpp, .h)↓
                             Preprocessing  Step 1: Preprocessor (cpp)
Include Header, Expand Macro(.i, .ii)↓
                              Compilation   Step 2: Compiler (gcc, g++)
                    Assembly Code(.s)↓
                                Assemble    Step 3: Assembler (as)
               Machine Code(.o, .obj)↓
        Static Library(.lib, .a)→Linking    Step 4: Linker (ld)
        Executable Machine Code(.exe)↓
```

GCC 将C/C++ 程序编译成可执行的程序，需要 4 个步骤，例如，`gcc -o hello.exe hello.c` 的执行情况如下:

1. 预处理: 通过 GNU C 预处理器(cpp.exe)，它包括头文件(#include)和扩展宏(#define)。

```shell
cpp hello.c > hello.i
```

生成的中间文件 "hello.i" 包含扩展的源代码。

2. 编译: 编译器将预处理的源代码编译成特定处理器的汇编代码。

```shell
gcc -S hello.i
```

`-S` 选项指定生成汇编代码，而不是对象代码。生成的程序集文件是 "hello.s"。

3. 汇编: 汇编程序(as.exe)将汇编代码转换为目标文件 "hello.o" 中的机器码。

```shell
as -o hello.o hello.s
```

4. 链接器: 最后，链接器(ld.exe)将目标代码与库代码链接起来，生成一个可执行文件 "hello.exe"。

### 内部库

库是预编译对象文件的集合，可以通过链接器链接到程序中。例如系统函数 `printf()` 和 `sqrt()`。内部库主要是是指头文件(.h)由自己实现。

### 外部库

外部库有两种类型: **静态库** 和 **共享库**。

1. 静态库的扩展名在 *nix 上为 `.a` (archive file), 在 Windows 上为 `.lib` (library)。当程序链接到静态库时，程序中使用的外部函数的机器码将复制到可执行文件中。静态库可以通过存档程序 "ar.exe" 创建。
2. 共享库的扩展名在 *nix 上为 `.so` (shared objects), 在 Windows 上为 `.dll` (dynamic link library)。当程序链接到共享库时，只在可执行文件中创建一个小表。在可执行文件开始运行之前，操作系统加载外部函数所需的机器码 —— 这是一个称为 **动态链接** 的过程。动态链接使可执行文件更小，并节省磁盘空间，因为一个库的一个副本可以在多个程序之间共享。此外，大多数操作系统允许内存中共享库的一个副本被所有正在运行的程序使用，从而节省内存。共享库代码可以在不需要重新编译程序的情况下进行升级。

由于动态链接的优点，默认情况下，如果共享库可用，GCC 将链接到共享库。

您可以通过 `nm filename` 列出库的内容。

`ldd hello.exe` 展示动态链接库列表

# 基础

```c
#include <stdio.h>   // 提供 printf()
#include <stdlib.h>  // 提供 system()

// C 标准, 没有参数时使用 void 关键字
int main(void) 
{
    printf("hello world");

    system("pause");
    // return 不写时, 编译器会自动添加
    return 0;
}
```

## 注释

在**预处理阶段**前被**移除**

```c
/* 多行注释 */
/*
 *
 */

// 单行注释(C99)
```

## 指针

```c
// 普通指向整型的指针
int *pi;
// 指向整型常量的指针,可以修改指针的值,但不能修改它所指向的值
int const *pci;
// 指向整型的常量指针,此时指针是常量,它的值无法修改,但可以修改它所指向的整型的值
int * const cpi;
// 指向整型常量的常量指针,不允许修改
int const * const cpci;
```

## 宏

```c
#include <stdio.h>

// 编译时期 被替换
#define sum(a, b) ((a)+(b))

int main(void)
{
    printf("%d\n", sum(1, 2)); // ((1)+(2))
    return 0;
}
```

# 资源

[cppreference.com](https://en.cppreference.com/w/)<br>
[【C++】预编译、编译、汇编、链接](https://blog.csdn.net/weixin_40740059/article/details/84075653)<br>
[C语言：春节回家过年，我发现只有我没有对象！](https://mp.weixin.qq.com/s/QT6abxt5evqxU8kDuhfdHQ)<br>