---
title: flutter基础语法：数值类型
sidebar_position:  4
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter数值类型']
---

import Image from '@theme/IdealImage';


 _Dart_ 里数值类型分为两种，一种是`int`，一种是`double`，它们是`num`的子类。如果一个数它不包含小数点，那它就属于`int`，否则它属于`double`。例如：

    void main() {
      var x = 1;
      var hex = 0xDEADBEEF;
      var y = 1.1;
      var exponents = 1.42234e3;
      print(x.runtimeType);
      print(hex.runtimeType);
      print(y.runtimeType);
      print(exponents.runtimeType);
    }


<Image img={require('./asserts/flutter5.png')} alt="运行结果" /><br />

 用`num`声明的变量，可以给它赋整数和小数值，例如：

    void main() {
      num i = 1;
      print(i.runtimeType);
      i += 2.5;
      print(i.runtimeType);
    }

<Image img={require('./asserts/flutter6.png')} alt="运行结果" /><br />

 `int`类型支持左移(`<<`)、右移(`>>`)、无符号右移(`>>>`)、按位非(`~`)、按位与(`&`)、按位或(`|`)和按位异或(`^`)。例如：

    assert((3 << 1) == 6); // 0011 << 1 == 0110
    assert((3 | 4) == 7); // 0011 | 0100 == 0111
    assert((3 & 4) == 0); // 0011 & 0100 == 0000

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
