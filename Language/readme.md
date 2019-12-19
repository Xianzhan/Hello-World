<!-- TOC -->

- [计算机的层次与编程语言](#计算机的层次与编程语言)
    - [硬件逻辑层](#硬件逻辑层)
    - [微程序机器层](#微程序机器层)
    - [传统机器层](#传统机器层)
    - [操作系统层](#操作系统层)
    - [汇编语言层](#汇编语言层)
    - [高级语言层](#高级语言层)
    - [应用层](#应用层)
- [诞生](#诞生)
- [资源](#资源)

<!-- /TOC -->

# 计算机的层次与编程语言

![](../.resource/Language/readme/层次.png)

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

# 诞生

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

# 资源

[Hello World的秘密](https://mp.weixin.qq.com/s/n9wJBVWPLoIlJox3lqQyVg)<br>
