---
title: flutter基础语法：操作符
sidebar_position:  10
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter操作符']
---

import Image from '@theme/IdealImage';

#### 1. 算术运算符

 _Dart_ 支持的算术运算符如下：

|   +   |              加             |
| :---: | :------------------------: |
|   -   |              减             |
|   \*  |              乘             |
|   /   |              除             |
|   ~/  |           除，返回整数           |
| -expr |         单目运算符，符号取反         |
|   %   |             求模             |
| var++ |  var = var + 1(表达式的值为var)  |
| ++var | var = var + 1(表达式的值为var+1) |
| --var | var = var - 1(表达式的值为var-1) |
| var-- |  var = var - 1(表达式的值为var)  |

    assert(2 + 3 == 5);
    assert(2 - 3 == -1);
    assert(2 * 3 == 6);
    assert(5 / 2 == 2.5); // Result is a double
    assert(5 ~/ 2 == 2); // Result is an int
    assert(5 % 2 == 1); // Remainder

    assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');

#### 2. 比较运算符

|   ==  |  等于  |
| :---: | :--: |
|   !=  |  不等于 |
|   >   |  大于  |
|  &lt; |  小于  |
|   >=  | 大于等于 |
| &lt;= | 小于等于 |

 _Dart_ 里一切都是对象，用操作符`==`判断两个对象是否相等。判断逻辑如下：

1.  如果 _x_ 和 _y_ 都为空，则返回`ture`；如果 _x_ 和 _y_ 只有一个为空，则返回`false`；如果两者都不为空，则进入逻辑2。

2.  调用 _x_ 的`==`的方法，传入的参数为 _y_。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
