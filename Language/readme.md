<!-- TOC -->

- [计算机的层次与编程语言](#计算机的层次与编程语言)
    - [硬件逻辑层](#硬件逻辑层)
    - [微程序机器层](#微程序机器层)
    - [传统机器层](#传统机器层)
    - [操作系统层](#操作系统层)
    - [汇编语言层](#汇编语言层)
    - [高级语言层](#高级语言层)
    - [应用层](#应用层)
- [发展](#发展)
    - [第一代语言](#第一代语言)
    - [第二代语言](#第二代语言)
    - [第三代语言](#第三代语言)
        - [诞生](#诞生)
- [编程语言](#编程语言)
    - [Python](#python)
    - [Golang](#golang)
- [资源](#资源)

<!-- /TOC -->

# 计算机的层次与编程语言

```sh
                   Application layer             -+
                +- High-level language layer      | Virtual machine
system software |  Assembly language layer        |
                +- Operating system layer        -+
                   Traditional machine layer     -+
                   Microprogramming machine layer | Actual machine
                   Hardware logic layer          -+
```

## 硬件逻辑层

- 门、触发器等逻辑电路组成
- 属于电子工程的领域

## 微程序机器层

- 编程语言是**微指令集**
- 微指令所组成的微程序直接交由硬件执行
- 由生产硬件公司编写

## 传统机器层

- 编程语言是 CPU 指令集(机器指令)
- 编程语言和硬件是直接相关
- 不同架构的 CPU 使用不同的 CPU 指令集
- Intel
- AMD

机器指令与微程序的关系:

1. 一条机器指令对应一个微程序
2. 一个微程序对应一组微指令

即: 微指令 < 微程序 = 机器指令

## 操作系统层

- 向上提供了简易的操作界面
- 向下对接了指令系统, 管理硬件资源
- 操作系统层是在软件和硬件之间的适配层

## 汇编语言层

- 编程语言是汇编语言
- 汇编语言可以翻译成可直接执行的机器语言
- 完成翻译过程的程序就是汇编器

```asm
PUSH DS
PUSH AX
MOV AX,0040
MOV DS,AX
```

## 高级语言层

- 编程语言为广大程序员所接受的高级语言
- 高级语言的类别非常多, 有几百种
- 常见的高级语言有: Python, Java, C/C++, Golang...

## 应用层

- 满足计算机针对某种用途而专门设计的<br>
    如: Excel<br>

# 发展

## 第一代语言

机器语言

## 第二代语言

汇编语言

## 第三代语言

### 诞生

[Family](https://ccrma.stanford.edu/courses/250a-fall-2005/docs/ComputerLanguagesChart.png)<br>

```shell
1954 Fortran
1958 Lisp
1964 Basic
1967 BCPL
1969 B
1970 Pascal
1971 C
1975 Scheme
1980 C with Classes
1983 C++ Objective-C
1987 Haskell Perl
1989 Bash
1991 Java Python
1993 Ruby
1995 JavaScript
1997 J
2000 C#
```

**面向过程语言**

- Fortran
- Basic
- C
- Pascal

**面向对象语言**

- Alog
- Simula67
- Ada
- SmallTalk
- C++
- Java
- C#

# 编程语言

> 1. 机器语言是直接通过十六进制数表示当前处理器架构的机器指令码。指令码包含了当前指令的功能（比如算术逻辑运算、移位、分支、中断、I/O 等）、寄存器、立即数等多种元素。每种处理器架构所对应的机器码的字节长度也各不相同，有些是固定长度的（比如 ARM、MIPS 等架构），有些是可变长度的（比如 x86 架构）。
> 2. 汇编语言（Assembly Language）通过简单的指令助记符（memonics）来表示对应机器指令的功能、寄存器编号、立即数（immediates）等元素。汇编语言是对机器指令的简单抽象，通过汇编器（assembler）可以将汇编语句翻译成对应的机器指令码。
> 3. 高级语言的表达形式更为抽象且贴近我们日常的语言表述。而且，高级语言比起汇编语言往往更具有表达力，且拥有更加丰富的语法特性，以便将程序进行结构化和模块化。比如，高级语言具有自定义变量标识符、自定义数据结构、分支与循环、更形象自然的表达式等。高级语言一般通过编译器（compiler）可直接将表达式翻译为对应的机器指令码；也可以将高级语言先翻译为中间语言（类似于汇编，但可能比汇编适用范围更广、更利于跨平台的字节码），最后将中间语言翻译为最终的机器指令码。

## Python

[Python 官网](https://www.python.org/)<br>

## Golang

[Golang 官网](https://golang.org/)<br>
[Golang 官网 CN](https://golang.google.cn/)<br>

# 资源

[Hello World的秘密](https://mp.weixin.qq.com/s/n9wJBVWPLoIlJox3lqQyVg)<br>
