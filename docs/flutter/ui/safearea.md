---
sidebar_position:  1
title: flutter界面设计：SafeArea
toc_max_heading_level: 4

keywords: ['flutter介绍','移动端编程','react native']
---

import Image from '@theme/IdealImage';

 移动设备顶部一般会有状态栏或者特殊的刘海，在设计 _ui_ 时需要防止内容被盖住。_flutter_ 提供了`SafeArea`解决这类问题。

1.  内容被覆盖的例子


    import 'package:flutter/material.dart';

    void main() {
      runApp(const MaterialApp(
        title: "test",
        home: const Text("来试试显示的问题，部分文字会被状态栏盖住"),
      ));
    }

<Image img={require('./asserts/flutter1.png')} alt="下载安装flutter插件" /> <br />

2.  使用SafeArea


    import 'package:flutter/material.dart';

    void main() {
      runApp(const MaterialApp(
        title: "test",
        home: const SafeArea(child: const Text("来试试显示的问题，部分文字会被状态栏盖住")),
      ));
    }


<Image img={require('./asserts/flutter2.png')} alt="下载安装flutter插件" /> <br />


 第一个例子没有使用`SafeArea`，文字被状态栏盖住了，第二个就没有这个问题。

* * *

1.  [SafeArea class](https://api.flutter.dev/flutter/widgets/SafeArea-class.html)

2.  [Flutter - Using SafeArea Widget Examples](https://www.woolha.com/tutorials/flutter-using-safearea-widget-examples)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
