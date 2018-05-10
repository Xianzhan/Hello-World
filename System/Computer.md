[硬件](#硬件)

[体系架构](#体系架构)

[程序设计语言](#程序设计语言)

[编译器](#编译器)

[数据结构和算法](#数据结构和算法)

[软件工程](#软件工程)

[resources](#resources)

![](https://raw.githubusercontent.com/Xianzhan/resources/master/pictures/20180425152407.png)

# 硬件

逻辑门; <br>
布尔运算; <br>
multiplexor (多路复用器); <br>
触发器 (flip-flop); <br>
寄存器 (register); <br>
RAM 单元; 计数器; <br>
硬件描述语言 (HDL, Hardware Description Language); <br>
芯片的仿真及测试.<br>

计算机最关键的三个部分：CPU,内存,I/O控制芯片。

![](https://raw.githubusercontent.com/Xianzhan/resources/master/pictures/20180412105858.png)

# 体系架构

内存管理; <br>
数学计算程序库; <br>
基本 I/O 驱动程序; <br>
屏幕管理; <br>
文件 I/O; <br>
对高级语言的支持.<br>

[【底层原理】基本内存管理（上）](https://mp.weixin.qq.com/s/MGEMmrCxTfi8K8spebsC_w)<br>
[【系统编程】高性能网络I/O入门（一）](https://mp.weixin.qq.com/s/IUlwCPvf8okbHsbfd1q2rA)

# 程序设计语言

基于对象 (object-based) 的设计和编程模式; <br>
抽象数据类型; <br>
作用域; <br>
语法和语义; <br>
引用 (reference) 机制.<br>

# 编译器

词法分析; <br>
自顶向下的语法分析; <br>
符号表 (symbol table); <br>
基于堆栈 (stack-based) 的虚拟机; <br>
代码生成; <br>
数组和对象的实现.<br>

![](https://raw.githubusercontent.com/Xianzhan/resources/master/pictures/20180503102918.png)

**预处理阶段** 预处理器 (cpp) 根据以字符 `#` 开头的命令, 修改原始的 C 程序. 比如 hello.c 中第一行的 `#include <stdio.h>` 命令告诉预处理器读取系统头文件 stdio.h 的内容, 并把它直接插入到程序文本中. 结果就得到另一个 C 程序, 通常是以 .i 作为文件扩展名.<br>
**编译阶段** 编译器 (cc1) 将文本文件 hello.i 翻译成文本文件 hello.s, 它包含一个*汇编语言程序*. 汇编语言程序中的每条语句都以一种标准的文本格式确切地描述了一条低级机器语言指令. 汇编语言是非常有用的, 因为它为不同高级语言的不同编译器提供了通用的输出语言. 例如, C 编译器和 Fortran 编译器产生的输出文件用的都是一样的汇编语言.<br>
**汇编阶段** 接下来, 汇编器 (as) 将 hello.s 翻译成机器语言指令, 把这些指令打包成一种叫做*可重定位的目标程序 (relocatable object program)* 的格式, 并将结果保存在目标文件 hello.o 中. hello.o 文件是一个二进制文件, 它的字节编码是机器语言指令而不是字符. 如果我们在文件编辑器中打开 hello.o 文件, 看到的将是一堆乱码.<br>
**链接阶段** 请注意, hello 程序调用了 `printf()` 函数, 它是每个 C 编译器都会提供的标准 C 库中的一个函数. `printf()` 存在于一个名为 printf.o 的单独的预编译好的目标文件中, 而这个文件必须以某种方式合并到我们的 hello.o 程序中. 连接器 (ld) 就负责处理这种合并. 结果就得到 hello 文件, 它是一个*可执行目标文件 (或简称为可执行文件)*, 可以被加载到内存中, 由系统执行.

# 数据结构和算法

堆栈; <br>
哈希表; <br>
链表; <br>
递归; <br>
算术算法; <br>
几何算法; <br>
运行效率.<br>

# 软件工程

模块化设计; <br>
接口/实现范式; <br>
API 设计和文档; <br>
主动式测试 (即极限编程理论中的单元测试等); <br>
广义的程序设计概念; <br>
质量保证体系.<br>

# resources

[程序员的自我修养：温故而知新](https://mp.weixin.qq.com/s/8rQKJxFaFDznrTRHmVNNQA)
