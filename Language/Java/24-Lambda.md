<!-- TOC -->

- [Lambda](#lambda)
- [方法引用](#方法引用)

<!-- /TOC -->

# Lambda

> @since 8

Lambda 允许把函数作为一个方法的参数，或者把代码看成数据。

```java
Runnable run = () -> System.out.println("hello");
```

# 方法引用

直接引用已有 Java 类或对象的方法。一般有四种不同的方法引用:

1. 构造器引用。语法是 `Class::new`，或者更一般的 `Class<T>::new`，要求构造器方法是没有参数；
2. 静态方法引用。语法是 `Class::static_method`，要求接受一个 Class 类型的参数；
3. 特定类的任意对象方法引用。它的语法是 `Class::method`。要求方法是没有参数的；
4. 特定对象的方法引用，它的语法是 `instance::method`。要求方法接受一个参数，与 3 不同的地方在于，3 是在列表元素上分别调用方法，而 4 是在某个对象上调用方法，将列表元素作为参数传入；