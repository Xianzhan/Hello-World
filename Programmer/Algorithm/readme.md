<!-- TOC -->

- [算法](#算法)
    - [复杂度分析](#复杂度分析)
        - [大 O 复杂度表示法](#大-o-复杂度表示法)
- [基本思想](#基本思想)
    - [分治](#分治)
        - [递归](#递归)
- [排序](#排序)
    - [冒泡排序](#冒泡排序)
    - [插入排序](#插入排序)
    - [希尔排序](#希尔排序)
    - [选择排序](#选择排序)
    - [归并排序](#归并排序)
    - [快速排序](#快速排序)
    - [桶排序](#桶排序)
    - [计数排序](#计数排序)
    - [基数排序](#基数排序)
    - [堆排序](#堆排序)
- [查找](#查找)
    - [二分查找](#二分查找)
- [搜索](#搜索)
    - [广度优先搜索(BFS)](#广度优先搜索bfs)
    - [深度优先搜索(DFS)](#深度优先搜索dfs)
- [消息摘要](#消息摘要)
    - [哈希算法](#哈希算法)
- [字符串匹配](#字符串匹配)
    - [BF 算法](#bf-算法)
    - [RK 算法](#rk-算法)
    - [BM 算法](#bm-算法)
- [数据结构](#数据结构)
- [线性表](#线性表)
    - [数组](#数组)
    - [链表](#链表)
    - [栈](#栈)
    - [队列](#队列)
        - [阻塞队列](#阻塞队列)
        - [并发队列](#并发队列)
    - [跳表(skip list)](#跳表skip-list)
    - [散列表](#散列表)
        - [散列冲突](#散列冲突)
- [树](#树)
    - [二叉树](#二叉树)
        - [满二叉树](#满二叉树)
        - [完全二叉树](#完全二叉树)
        - [二叉查找树](#二叉查找树)
        - [平衡二叉树](#平衡二叉树)
        - [平衡二叉查找树](#平衡二叉查找树)
            - [AVL 树](#avl-树)
            - [红黑树](#红黑树)
    - [堆](#堆)
- [图](#图)
- [资源](#资源)

<!-- /TOC -->

# 算法

算法就是操作数据的一组方法, 作用在特定的数据结构之上.

1. 复杂度分析
    1. 空间复杂度
    2. 时间复杂度
        1. 最好
        2. 最坏
        3. 平均
        4. 均摊
2. 基本算法思想
    1. 贪心算法
    2. 分治算法
    3. 动态规划
    4. 回溯算法
    5. 枚举算法
3. 字符串匹配
    1. 朴素
    2. KMP
    3. Robin-Karp
    4. Boyer-Moore
    5. AC 自动机
    6. Trie
    7. 后缀数组
4. 查找
    1. 线性表查找
    2. 树结构查找
    3. 散列表查找
5. 排序
    1. O(n^2)
        1. 冒泡排序
        2. 插入排序
        3. 选择排序
        4. 希尔排序
    2. O(nlogn)
        1. 归并排序
        2. 快速排序
        3. 堆排序
    3. O(n)
        1. 计数排序
        2. 基数排序
        3. 桶排序
6. 其它
    1. 数论
    2. 计算几何
    3. 概率分析
    4. 并查集
    5. 拓扑网络
    6. 矩阵运算
    7. 线性规划

## 复杂度分析

复杂度分析是整个算法学习的精髓, 只要掌握了它, 数据结构和算法的内容基本上就掌握了一半.

### 大 O 复杂度表示法

大 O 时间复杂度实际上并不具体表示代码真正的执行时间, 而是表示**代码执行时间随数据规模增长的变化趋势**, 所以, 也叫作**渐进时间复杂度**(asymptotic time complexity), 简称**时间复杂度**.

**多项式量级**

- 常量阶 O(1)

一般情况下, 只要算法不存在循环语句、递归语句, 即使有成千上万行的代码, 其时间复杂度也是 O(1).

```java
int i = 1;
int j = 2;
int sum = i + j;
```

- 对数阶 O(logn)

```java
int i = 1;
while (i <= n) {
    i = i * 2;
}
```

- 线性阶 O(n)

```java
int i = 0;
while (i < n) {
    i++;
}
```

- 线性对数阶 O(nlogn)
- 平方阶 O(n^2)、立方阶 O(n^3)、k 次方阶 O(n^k)

**非多项式量阶**

- 指数阶 O(2^n)
- 阶乘阶 O(n!)

# 基本思想

## 分治

### 递归

分治是一种解决问题的处理思想, 递归是一种编程技巧.

**递归需要满足的三个条件**

1. 一个问题的解可以分解为几个子问题的解.
2. 这个问题与分解之后的子问题, 除了数据规模不同, 求解思路完全一样.
3. 存在递归终止条件.

*递归代码要警惕栈溢出*, *递归代码要警惕重复计算*

# 排序

排序算法|时间复杂度|稳定|原地
:---:|:---:|:---:|:---:
冒泡排序|O(n^2)|✔|✔
插入排序|O(n^2)|✔|✔
选择排序|O(n^2)|×|✔
快速排序|O(nlogn)|×|✔
归并排序|O(nlogn)|✔|×
计数排序|O(n+k)k是数据范围|✔|×
桶排序|O(n)|✔|×
基数排序|O(dn)d是维度|✔|×

[终于，把十大经典排序算法汇总了！（Java实现版）](https://mp.weixin.qq.com/s/xSm4Gutudq1VdKuXGFoDMg)<br>
[十大经典排序算法动画与解析，看我就够了！（配代码完全版）](https://mp.weixin.qq.com/s/vn3KiV-ez79FmbZ36SX9lg)<br>

## 冒泡排序

冒泡排序只会操作相邻的两个数据. 每次冒泡至少会让一个元素移动到它应该在的位置.

## 插入排序

将数据分为两个区间, 已排序区间和未排序区间. 取未排序区间的元素, 在已排序区间中找到合适的插入位置将其插入.

## 希尔排序

[漫画：什么是希尔排序？](https://mp.weixin.qq.com/s/b9-dkpAhWJYshuSs5cwnOw)<br>

## 选择排序

选择排序算法的实现思路有点类似插入排序, 也分已排序区间和未排序区间. 但是选择排序每次会从未排序区间找到最小元素, 将其放到已排序区间的末尾.

## 归并排序

先把数组从中间分成前后两部分, 然后对前后两部分分别排序, 再将排好序的两部分合并在一起.

## 快速排序

将数据分为三部分, 第一部分是都比分割数据小的, 第二部分是分割数据, 第三部分是都比分割数据大的. 如此反复.

1. 从无序的数据里挑出第 K 大的位置

## 桶排序

首先, 排序的数据可以划分成 m 个桶, 并且, 桶与桶之间有着天然的大小顺序. 其次, 数据在各个桶之间的分布是比较均匀的.

**桶排序比较适合用在外部排序.**

## 计数排序

计数排序其实是桶排序的一种特殊情况. 当要排序的 n 个数据, 所处的范围并不大的时候, 比如最大值是 k, 我们可以把数据划分为 k 个桶, 每个桶内的数据值都是相同的, 省掉桶内排序的时间.

1. 计算高考分数的排名.

## 基数排序

基数排序对要排序的数据是有要求的，需要可以分割出独立的 “位” 来比较，而且位之间有递进的关系，如果 a 数据的高位比 b 数据大，那剩下的低位就不用比较了。除此之外，每一位的数据范围不能太大，要可以用线性排序算法来排序，否则，基数排序的时间复杂度就无法做到 O(n) 了

1. 手机号排序
2. 英语字典单词排序

## 堆排序

堆排序对应的是数据结构是堆, 是一种原地的、时间复杂度为 O(nlogn) 的排序算法.

# 查找

## 二分查找

针对有序数据集合的查找算法.

# 搜索

## 广度优先搜索(BFS)

## 深度优先搜索(DFS)

# 消息摘要

## 哈希算法

将任意长度的二进制值串映射为固定长度的二进制串.

1. 安全加密
2. 唯一标识
3. 数据校验
4. 散列函数

**分布式相关**

5. 负载均衡
6. 数据分片
7. 分布式存储

# 字符串匹配

## BF 算法

Brute Force 的缩写, 中文叫作暴力匹配算法, 也叫朴素匹配算法.

简单场景, 主串和模式串都不太长, O(m*n)

## RK 算法

RK 算法的全称叫 Rabin-Karp 算法，是由它的两位发明者 Rabin 和 Karp 的名字来命名的.

计算子串与部分主串的哈希值是否一致.

## BM 算法

Boyer-Moore 算法

<!-- TOC -->

- [数据结构](#数据结构)
- [线性表](#线性表)
    - [数组](#数组)
    - [链表](#链表)
    - [栈](#栈)
    - [队列](#队列)
        - [阻塞队列](#阻塞队列)
        - [并发队列](#并发队列)
    - [跳表(skip list)](#跳表skip-list)
    - [散列表](#散列表)
        - [散列冲突](#散列冲突)
- [树](#树)
    - [二叉树](#二叉树)
        - [满二叉树](#满二叉树)
        - [完全二叉树](#完全二叉树)
        - [二叉查找树](#二叉查找树)
        - [平衡二叉树](#平衡二叉树)
        - [平衡二叉查找树](#平衡二叉查找树)
            - [AVL 树](#avl-树)
            - [红黑树](#红黑树)
    - [堆](#堆)
- [图](#图)
- [资源](#资源)

<!-- /TOC -->

# 数据结构

数据结构就是指一组数据的存储结构, 是为算法服务的.

1. 线性表
    1. 数组
    2. 链表
        1. 单链表
        2. 双向链表
        3. 循环链表
        4. 双向循环链表
        5. 静态链表
    3. 栈
        1. 顺序栈
        2. 链式栈
    4. 队列
        1. 普通队列
        2. 双端队列
        3. 阻塞队列
        4. 并发队列
        5. 阻塞并发队列
2. 散列表
    1. 散列函数
    2. 冲突解决
        1. 链表法
        2. 开发寻址
    3. 动态扩容
    4. 位图
3. 树
    1. 二叉树
        1. 平衡二叉树
        2. 二叉查找树
        3. 平衡二叉查找树
            1. AVL 树
            2. 红黑树
        4. 完全二叉树
        5. 满二叉树
    2. 多路查找树
        1. B 树
        2. B+ 树
        3. 2-3 树
        4. 2-3-4 树
    3. 堆
        1. 小顶堆
        2. 大顶堆
        3. 优先级队列
        4. 斐波那契堆
        5. 二项堆
    4. 其它
        1. 树状数组
        2. 线段树
4. 图
    1. 图的存储
        1. 邻接矩阵
        2. 邻接表
    2. 拓扑排序
    3. 最短路径
    4. 关键路径
    5. 最小生成树
    6. 二分图
    7. 最大流


# 线性表

## 数组

**特性**: 连续的内存空间和相同类型的数据.

对 CPU 缓存友好, 可以有效预读.

为什么大多数编程语言中, 数组下标要从 0 开始编号, 而不是从 1 开始呢?

从数组存储的内存模型上来看, "下标" 最确切的定义应该是 "偏移(offset)". arr[0] 就是偏移为 0 的位置.

## 链表

类型|数组|链表
:---:|:---:|:---:
插入删除|O(n)|O(1)
随机访问|O(1)|O(n)

与数组相比, 无需像数组那样需要申请连续的内存空间, 它通过 "指针" 将一组零散的内存块串联起来使用.

应用场景

LRU 缓存淘汰算法

1. 如果此数据之前已经被缓存在链表中, 我们遍历得到这个数据对应的节点, 并将其从原来的位置删除, 然后再插入到链表的头部.
2. 如果此数据没有在缓存链表中, 又可以分为两种情况:
    - 如果此时缓存未满, 则将此节点直接插入到链表的头部;
    - 如果此时缓存已满, 则链表尾节点删除, 将新的数据节点插入链表的头部.

缓存是一种提高数据读取性能的技术, 在硬件设计、软件开发中都有非常广泛的应用, 比如常见的 CPU 缓存、数据库缓存、浏览器缓存等等.

## 栈

从栈的操作特性上来看, 栈是一种 "操作受限" 的线性表, 只允许在一端插入和删除数据.

用数组实现的栈叫做顺序栈, 用链表实现的栈叫作链式栈.

应用场景

1. 函数调用栈
2. 编译器利用栈来实现表达式求值
3. 浏览器的前进和后退, 使用两个栈

## 队列

先进者先出.

用数组实现的队列叫作顺序队列, 用链表实现的队列叫作链式队列.

### 阻塞队列

- 在队列为空的时候, 从队头取数据会被阻塞, 直到队列中有了数据才能返回;
- 如果队列已经满了, 那么插入数据的操作就会被阻塞, 直到队列中有空闲位置后再插入数据.

### 并发队列

线程安全的队列我们叫作并发队列.

基于链表的实现方式, 可以实现一个支持无限排队的无界队列(unbounded queue), 但是可能会导致过多的请求排队等待, 请求处理的响应时间过长. 所以, 针对响应时间比较敏感的系统, 基于链表实现的无限排队的线程池是不合适的.

基于数组实现的有界队列(bounded queue), 队列的大小有限, 所以线程池中排队的请求超过队列大小时, 接下来的请求就会被拒绝, 这种方式对响应时间敏感的系统来说, 就相对更加合理. 不过, 设置一个合理的队列大小, 也是非常有讲究的. 队列太大导致等待的请求太多, 队列太小会导致无法充分利用系统资源, 发挥最大性能.

## 跳表(skip list)

链表加多级索引的结构.

1. redis 的有序集合(sorted set)

## 散列表

散列表用的是数组支持按照下标随机访问数据的特性, 所以散列表其实就是数组的一种扩展, 由数组演化而来. 可以说, 如果没有数组, 就没有散列表.

1. word 文档中单词的拼写检查功能.

### 散列冲突

解决方法:

1. 开放寻址法(open addressing)

如果出现散列冲突, 就重新探测一个空闲位置, 将其插入.

2. 链表法(chaining)

如果出现散列冲突, 则冲突位置转为一个链表记录. 如果散列算法不好, 则时间复杂度变为 O(n).

# 树

## 二叉树

概念: 高度(height)、深度(depth)、层(level)

- 节点的高度 = 节点到叶子节点的最长路径
- 节点的深度 = 根节点到这个节点所经历的边的个数
- 节点的层数 = 节点的深度 + 1
- 树的高度 = 根节点的高度

遍历

- 前序遍历: 对于树中的任意节点, 先打印这个节点, 然后再打印它的左子树, 最后打印它的右子树.
- 中序遍历: 对于树中的任意节点, 先打印它的左子树, 然后再打印它本身, 最后打印它的右子树.
- 后序遍历: 对于树中的任意节点, 先打印它的左子树, 然后再打印它的右子树, 最后打印这个节点本身.

### 满二叉树

叶子节点都在最底层, 除了叶子节点之外, 每个节点都有左右两个子节点.

### 完全二叉树

叶子节点都在最底下两层, 最后一层的叶子节点都靠左排列, 并且除了最后一层, 其它层的节点个数都要达到最大.

1. 堆是一种完全二叉树, 最常用的存储方式就是数组.

### 二叉查找树

也叫二叉搜索树, 是为实现快速查找而生的. 不仅仅支持快速查找一个数据, 还支持快速插入、删除一个数据.

特性:

1. 左子树的值一定小于当前节点的值, 而右子树的值都大于这个节点的值.

### 平衡二叉树

二叉树中任意一个节点的左右子树的高度相差不能大于 1. 完全二叉树和满二叉树都是平衡二叉树.

### 平衡二叉查找树

#### AVL 树

任何节点的左右子树高度相差不超过 1, 是一种高度平衡的二叉查找树.

AVL 树是一种高度平衡的二叉树, 所以查找的效率非常高, 但是, 每次插入、删除都要做调整.

#### 红黑树

红黑树的插入、删除、查找各种操作性能都比较稳定.

特性:

1. 根节点是黑色的
2. 每个叶子节点都是黑色的空节点, 也就是说, 叶子节点不存储数据
3. 任何相邻的节点都不能同时为红色, 也就是说, 红色节点是被黑色节点隔开的
4. 每个节点, 从该节点到达其可达叶子节点的所有路径, 都包含相同数目的黑色节点

## 堆

堆适合使用数组来实现.

特性:

1. 堆是一个完全二叉树
2. 堆中每一个节点的值都必须大于等于(或小于等于)其子树中的每个节点的值.

使用场景:

1. 优先级队列
2. 利用堆求 Top K
3. 求中位数

# 图

图由顶点(vertex) 和边(edge) 构成.

存储方法:

1. 邻接矩阵(adjacency matrix)
2. 邻接表(adjacency list)

# 资源

[GitHub://algorithms](https://github.com/jeffgerickson/algorithms)<br>
[GitHub://algorithm-visualizer](https://github.com/algorithm-visualizer/algorithm-visualizer)<br>
[GitHub://LeetCodeAnimation](https://github.com/MisterBooo/LeetCodeAnimation)<br>
[GitHub://javascript-algorithms](https://github.com/trekhleb/javascript-algorithms)<br>
<算法图解><br>
[算法合集](https://mp.weixin.qq.com/s/76GU8JtWntEesWoARKzR9g)<br>
