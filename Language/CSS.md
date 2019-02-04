<!-- TOC -->

- [Base](#base)
    - [盒子模型](#盒子模型)
    - [选择器](#选择器)
    - [优先级](#优先级)

<!-- /TOC -->

# Base

## 盒子模型

![](https://raw.githubusercontent.com/Xianzhan/resources/master/pictures/20180331232258.png)

标准盒子模型: 宽度 = content + padding + border + margin

## 选择器

- id 选择器 (#id_name)
- 类选择器 (.class_name)
- 标签选择器 (div, p...)
- 相邻选择器 (p + p)
- 子选择器 (p > div)
- 后代选择器 (select option)
- 通配符选择器 (*)
- 属性选择器 (a[href="abc.com"])
- 伪类选择器 (a:hover, li:nth-child)

## 优先级

就近原则: !important > [id > class > tag]
