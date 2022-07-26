---
title: flutter基础语法：列表类型
sidebar_position:  6
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter列表类型']
---

import Image from '@theme/IdealImage';

#### 1. 定义

 列表是相同类型数据的序列，在 _Dart_ 里用`List`表示列表，`[]`来初始化。例如：

    var list = [1, 2, 3];

:::tip

 编译器推导`list`的类型为`List<int>`,这里涉及到泛型（_Generics_）。泛型会在后面进行介绍。

:::

#### 2. 获取列表元素

 `list`的下标从0开始，最后一个元素的下标为`list.length - 1`。它支持使用下标来访问列表里的元素，例如：

    var list = [1, 2, 3];
    assert(list.length == 3);
    assert(list[1] == 2);

    list[1] = 1;
    assert(list[1] == 1);


#### 3. 扩展操作符

 _Dart_ 2.3引入了扩展操作符(`...`)和适配空的扩展操作符(`...?`)，这两个操作符可以一次性向集合里插入多条数据。例如将一个列表的数据全部插入到另外一个列表中。

    var list = [1, 2, 3];
    var list2 = [0, ...list];
    assert(list2.length == 4);

 上面例子中，如果`list`可能为空，需要使用`...?`操作符。

#### 4. collection if 和 collection for

 除了扩展操作符，还有 _collection if_ 和 _collection for_ 语法，用来初始化列表，例如：

    var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
    var listOfInts = [1, 2, 3];
    var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
    assert(listOfStrings[1] == '#1');

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
