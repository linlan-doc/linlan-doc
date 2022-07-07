---
title: go字符串类型
sidebar_position:  6
keywords: ['go编程','go数据类型','go语言学习','go字符串类型']
toc_max_heading_level: 4
---

 _go_ 用 _string_ 来标识字符串类型，在了解字符串类型前，可以前往[基础语法](./basic-syntax)的 _String literals_ ，了解字符串常量的内容。

 字符串类型有以下特性：

1.  _string_ 底层是一个字节数组，是字符常量经过 _UTF-8_ 编码之后的结果。

2.  _string_ 是不可变更（_immutable_）的，这点和 _Java_ 一样。

3.  _string_ 类型没有方法，操作 _string_ 的方法由 _strings_ 包提供。

4.  在赋值语句中，源字符串和目标字符串共享底层的字节数组；通过 _substring_ 方法获得的子串和源字符串共享底层字节数组。

5. 空字符串可以用`""` 和 `` ` `  `` 来表示。 

### 1. 字符串操作

#### 1.1 运算符

 字符串支持的运算符有以下几种

    + += == != > < >= <=
