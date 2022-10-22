---
title: 如何实现背景颜色渐变效果
sidebar_position:  4
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter组件教程','flutter gradient']
---

import Image from '@theme/IdealImage';

 纯色背景会略显单调，本文介绍 _flutter_ 中如何实现背景颜色渐变效果。

<Image img={require('./asserts/flutter4.png')} alt="渐变背景" /> <br />

#### 1. LinearGradient

 _Gradient_ 用来实现 _flutter_ 里的渐变，它有以下几种实现：_LinearGradient_、_SweepGradient_ 和 _RadialGradient_ 。本文使用 _LinearGradient_ 来实现渐变效果。

  _LinearGradient_ 的构造函数如下，`begin`和`end`两个参数表示渐变开始和结束的地方，可以用它们来指定渐变的方向：如果希望水平方向，则从 _topLeft_ 到 _topRight_；如果希望竖直方向，则从 _topLeft_ 到 _bottomLeft_ 。除了使用 _Alignment_ 里定义的常量外，也可以使用 _FractionalOffset_ 指定任意的位置。

    LinearGradient({AlignmentGeometry begin = Alignment.centerLeft, AlignmentGeometry end = Alignment.centerRight, required List<Color> colors, List<double>? stops, TileMode tileMode = TileMode.clamp, GradientTransform? transform})

<Image img={require('./asserts/flutter5.png')} alt="竖直方向渐变" /> <br />

 `colors`是一个颜色数组，用来指定渐变使用的颜色。`stops`也是一个数组，它和颜色数组的长度一样，取值范围为[0,1]，用来表示对应每个颜色的分数。以本文红蓝渐变为例，`colors`的数组如下,红色在前，蓝色在后。如果设置`stops`值为[0.2,0.8]，则表示[0,0.2]之间为纯红色，[0.2,0.8]之间为红蓝渐变，[0.8,1]为纯蓝色。

    colors: [Colors.red, Colors.blue]

<Image img={require('./asserts/flutter6.png')} alt="设置stops值" /> <br />

 如果设置起点是 _topLeft_，终点是 _bottomRight_，`stops`设置为[0.5,0.5]，还能得到下面效果。

<Image img={require('./asserts/flutter7.png')} alt="特殊效果" /> <br />

 `tileMode`用来指定如何填充`begin`之前和`end`之后的内容，加入想要实现下图中的对称效果，可以将开始设置为`topCenter`，结束设置为`topRight`，`tileMode`设置为`TileMode.mirror`。

    decoration: const BoxDecoration(
                    gradient: LinearGradient(
                        colors: [Colors.red, Colors.blue],
                        begin: Alignment.topCenter,
                        end: Alignment.topRight,
                        stops: [0, 1],
                        tileMode: TileMode.mirror))

<Image img={require('./asserts/flutter8.png')} alt="tileMode示例" /> <br />

#### 2. 完整代码

    import 'package:flutter/material.dart';
    import 'package:google_fonts/google_fonts.dart';

    void main(List<String> args) {
      runApp(const MaterialApp(
        title: "Planets",
        home: HomePage(),
      ));
    }

    class HomePage extends StatelessWidget {
      const HomePage({Key? key}) : super(key: key);

      @override
      Widget build(BuildContext context) {
        return SafeArea(
          child: Scaffold(
            appBar: AppBar(
              leading: IconButton(
                icon: const Icon(Icons.menu),
                onPressed: () {},
              ),
              flexibleSpace: Container(
                decoration: const BoxDecoration(
                    gradient: LinearGradient(
                        colors: [Colors.red, Colors.blue],
                        begin: Alignment.topLeft,
                        end: Alignment.bottomRight,
                        stops: [0.5, 0.5],
                        tileMode: TileMode.clamp)),
              ),
              title: Center(
                child: Text(
                  "Gradient",
                  style: GoogleFonts.poppins(
                      fontWeight: FontWeight.w600, fontSize: 36.0),
                ),
              ),
              actions: [
                IconButton(onPressed: () {}, icon: const Icon(Icons.settings))
              ],
            ),
            body: Container(),
          ),
        );
      }
    }

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
