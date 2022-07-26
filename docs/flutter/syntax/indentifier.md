---
title: flutter基础语法：标识符
sidebar_position:  2
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter标识符']
---

import Image from '@theme/IdealImage';

 标识符是程序里变量、函数、类、方法等的名称，_Dart_ 规定标识符由字母、数字和特殊字符(`-`和`$`)组成。标识符的命名要求有：

1.  第一个字符不能是数字。
2.  不能使用关键字(例外的情况参考[flutter关键字](./keywords))。
3.  区分大小写。

 标识符命名有三种风格：

1.  _UpperCamelCase_ :每一个单词的首字母都大写。
2.  _lowerCamelCase_: 除了第一个单词，其他单词的首字母大写。
3.  _lowercase_with_underscores_ : 每一个单词都小写，单词使用`_`连接。

#### 1. 类、枚举、类型等使用 _UpperCamelCase_

    class SliderMenu { ... }

    class HttpRequest { ... }

    typedef Predicate<T> = bool Function(T value);

    class Foo {
      const Foo([Object? arg]);
    }

    @Foo(anArg)
    class A { ... }

    @Foo()
    class B { ... }

    extension MyFancyList<T> on List<T> { ... }

    extension SmartIterable<T> on Iterable<T> { ... }

#### 2. 库、包、目录、源文件名、导入前缀等使用 _lowercase_with_underscores_

    library peg_parser.source_scanner;

    import 'file_system.dart';
    import 'slider_menu.dart';
    import 'package:angular_components/angular_components.dart' as angular_components;

#### 3. 其他类型标识符使用 _lowerCamelCase_

    HttpRequest httpRequest;

    void alignItem(bool clearItems) {
      // ...
    }

#### 4. 首字母缩写当成常规单词

    class HttpConnection {}
    class DBIOPort {}
    class TVVcr {}
    class MrRogers {}

    var httpRequest = ...
    var uiHandler = ...
    var userId = ...
    Id id;

:::caution

 长度不超过2的全部大写，除了 _Id_ 。

:::

* * *

1.  [Dart Specification](https://dart.dev/guides/language/specifications/DartLangSpec-v2.10.pdf)
2.  [Dart – Basic Syntax](https://www.geeksforgeeks.org/dart-basic-syntax/)
3.  [Effective Dart: Style](https://dart.dev/guides/language/effective-dart/style)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
