---
title: flutter基础语法：变量
sidebar_position:  3
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter变量']
---

import Image from '@theme/IdealImage';

#### 1. 定义

 程序运行过程中的数据会存放在内存的某个地址上，变量是内存地址的名称，通过变量可以获取到对应的数据或者对数据进行修改。下面例子声明并初始化一个变量。

    var name = 'Bob';

 `var`声明的变量没有指定变量的类型，但编译器会推导出变量的类型，并进行替换，所以下面的代码无法通过编译。

    import 'package:flutter/material.dart';

    void main() {
      var test = "abc";
      print(test);
      test=1; //test的类型已经是String
      print(test.runtimeType);//打印test的类型
    }

<Image img={require('./asserts/flutter1.png')} alt="编译失败" /><br />

 声明变量时，可以显式的指定变量的类型。例如：

    String name = 'Bob';

#### 2. Object和dynamic

 如果并不限制变量的类型，可以使用`Object`或者`dynamic`。例如：

    void main() {
      Object test = "abc";
      print(test);
      test = 1;
      print(test);

      dynamic test2 = "def";
      print(test2);
      test2 = 1;
      print(test2);
    }

<Image img={require('./asserts/flutter2.png')} alt="执行结果" /><br />

 `dynamic`并不是一种类型，编译器不会进行类型检查。`Object`是一种类型，编译会进行类型检查。下面的代码无法通过编译。

    void main() {
      dynamic a = 2;
      Object b = 2;

      print(a.length);//编译成功
      print(b.length);//编译错误
    }

<Image img={require('./asserts/flutter3.png')} alt="编译报错" /><br />

 没有初始化的变量的默认值为`null`,因为 _Dart_ 里所有的类型都是对象，所以数值类型的默认值也是`null`。

    void main() {
      int? a;
      print(a);
    }

<Image img={require('./asserts/flutter4.png')} alt="运行结果" /><br />

:::tip

  _Dart_ 支持 _null safety_ ，即变量不能为 _null_，如果希望变量能为 _null_ ，在变量类型后面加上`?`。

:::

#### 3. late关键字

 因为[null safety](https://dart.dev/null-safety)， _Dart_ 会分析代码流程，确保变量在使用之前被赋予了一个非空值。但有些时候分析会失败，即使变量已经被赋予了非空值，编译器还是提示变量不能为空。_Dart_ 2.12加入了`late`关键字，告诉编译器这个变量非空。

    late String description;

    void main() {
      description = 'Feijoada!';
      print(description);
    }

 `late`关键字还意味着懒加载。如果在声明`late`变量时进行了初始化，那么初始化会推迟到变量第一次使用时执行。如果变量没有被使用，则初始化不会执行。

#### 4. final和const

 如果在代码执行过程中不允许改变变量，可以使用`final`或者`const`修饰变量。`final`的变量只能赋值一次（不要求在声明时），`const`变量必须是编译时常量（`const`变量默认是`final`）。

    final name = 'Bob'; // Without a type annotation
    name = 'Alice'; // Error: a final variable can only be set once.

    const bar = 1000000; // Unit of pressure (dynes/cm2)
    const double atm = 1.01325 * bar; // Standard atmosphere

 `final`和`const`的区别在于：`final`仅仅限制被`final`修饰的变量不能二次赋值，但变量里的内容是可以修改的，例如：

    void main() {
      final a = [1, 2, 3];
      a.add(4);
      print(a);
    }


<Image img={require('./asserts/flutter13.png')} alt="运行结果" /><br />

 如果将修饰符从`final`换成`const`，运行时会报错。

<Image img={require('./asserts/flutter14.png')} alt="运行结果" /><br />

* * *

1.  [What is the difference between dynamic and Object in dart?](https://stackoverflow.com/questions/31257735/what-is-the-difference-between-dynamic-and-object-in-dart)

2.  [language-tour](https://dart.dev/guides/language/language-tour#default-value)

3.  [What is the difference between the "const" and "final" keywords in Dart?](https://stackoverflow.com/questions/50431055/what-is-the-difference-between-the-const-and-final-keywords-in-dart)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
