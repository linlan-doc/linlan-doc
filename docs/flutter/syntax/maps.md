---
title: flutter map类型
sidebar_position:  8
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter map类型']
---

import Image from '@theme/IdealImage';

 _map_ 类型是计算机科学里非常重要的数据类型，它是键值对的集合。_map_ 里键只出现一次，值可以出现多次。_Dart_ 中 _map_ 的数据类型为 _Map_ ，使用`{}`进行初始化。例如：

    var gifts = {
      // Key:    Value
      'first': 'partridge',
      'second': 'turtledoves',
      'fifth': 'golden rings'
    };

:::tip

 _set_ 和 _map_ 初始化都使用`{}`，当初始化空的 _set_ 时，编译器默认将`{}`当作 _map_，所以声明 _set_ 需要指明类型。例如：

    var set = {};//编译器会将set推断成map
    Set<String> s = {};//需要指定变量的类型  

:::

 _map_ 可以通过键值对的键(_key_)来获取对应的值(_value_)。_Dart_ 里 _map_ 获取键对应值的语法和 _list_ 类似，使用`[key]`，例如：

    var gifts = {'first': 'partridge'};
    assert(gifts['first'] == 'partridge');


 如果对应的 _key_ 不存在，则返回`null`，例如：

    void main() {
      var map = {"hello": "world", "flutter": "dart"};
      var value = map['react'];
      print(value);
    }


<Image img={require('./asserts/flutter12.png')} alt="运行结果" /><br />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
