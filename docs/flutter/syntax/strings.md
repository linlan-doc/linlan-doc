---
title: flutter字符串类型
sidebar_position:  5
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter字符串类型']
---

import Image from '@theme/IdealImage';

#### 1. 定义

  _Dart_ 里 _String_ 是 _UTF-16_ 编码的码点序列。关于码点和编码方式，可以参考[go string类型](../../golang/syntax/basic-syntax#7-rune-literals)。_String_ 的创建可以通过单引号`'`或者双引号`"`完成。

    var s1 = 'Single quotes work well for string literals.';
    var s2 = "Double quotes work just as well.";
    var s3 = 'It\'s easy to escape the string delimiter.';
    var s4 = "It's even easier to use the other delimiter.";

#### 2. unicode表示

 对于很多非字母或者数字的字符，可以使用 _unicode_ 进行表示。例如😂的 _unicode_ 为`U+1F602`，它的声明方式如下。

    void main() {
      String a = "I am \u{1F602}";
      print(a);
    }

<Image img={require('./asserts/flutter7.png')} alt="运行结果" /><br />

:::tip

 如果字符的 _unicode_ 是4个16进制数，`{}`可以去掉。

:::

#### 3. 包含表达式

 在 _String_ 内部允许使用`${expression}`占位符来表示表达式的计算结果，如果`expression`是标识符，则`{}`可以省略，例如：

    var s = 'string interpolation';

    assert('Dart has $s, which is very handy.' ==
        'Dart has string interpolation, '
            'which is very handy.');
    assert('That deserves all caps. '
            '${s.toUpperCase()} is very handy!' ==
        'That deserves all caps. '
            'STRING INTERPOLATION is very handy!');

:::tip

 _String_ 是否相等比较的是两者是否拥有相同的码点序列。

:::

#### 4. raw string

 在字符串前面加上`r`表示 _raw string_ ，在 _raw string_ 里不会对特殊字符进行解析。例如：换行符`\n`。

    void main() {
      var s = r'In a raw string, not even \n gets special treatment.';
      var b = 'In a raw string, not even \n gets special treatment.';
      print(s);
      print(b);
    }

<Image img={require('./asserts/flutter8.png')} alt="运行结果" /><br />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
