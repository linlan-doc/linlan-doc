---
title: flutter set类型
sidebar_position:  7
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter set']
---

import Image from '@theme/IdealImage';

#### 1. 定义

 _set_ 类型是值唯一的数据集合。_Dart_ 里使用 _Set_ 来表示，通过`{}`来初始化。例如：

    var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};

#### 2. 向set添加元素

 通过调用`add`和`addAll`可以向 _set_ 里添加元素，因为值唯一，所以添加已经存在的元素会返回`false`。

    void main() {
      var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
      var result = halogens.add('fluorine');
      print(result);
    }

<Image img={require('./asserts/flutter9.png')} alt="运行结果" /><br />

#### 3. 扩展操作符

 和[列表](./lists)一样，_set_ 也支持扩展操作符，_collection if_ 和 _collection for_ 等语法，例如：

    void main() {
      var list = [1, 2, 3, 4, 5];
      var list2 = [12, 13, 14];
      var set = {9, ...list, for (var i in list2) i + 10};
      print(set);
    }

<Image img={require('./asserts/flutter10.png')} alt="运行结果" /><br />

#### 4. 判断元素是否存在

 _set_ 最常见的操作是：判断是否包含指定的元素。_Dart_ 里 _Set_ 提供了`Contains`方法，返回值为`bool`类型。例如：

    void main() {
      Set<String> stringSet = {"hello", "world", "flutter"};
      bool contains = stringSet.contains("hello");
      print(contains);
    }

<Image img={require('./asserts/flutter11.png')} alt="运行结果" /><br />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
