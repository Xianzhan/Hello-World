<!-- TOC -->

- [基础](#基础)
    - [指针](#指针)
    - [宏](#宏)
- [资源](#资源)

<!-- /TOC -->

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