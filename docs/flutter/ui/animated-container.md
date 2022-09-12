---
title: AnimatedContainer使用教程
sidebar_position:  3
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter动画','AnimatedContainer']
---

import Image from '@theme/IdealImage';

> 本文是Flutter动画系列的第二篇，建议读者阅读前面的教程，做到无缝衔接。

 [AnimatedOpacity](./implicity-animation.md)一文介绍了透明度动画，但`AnimatedOpacity`仅支持透明度，这显然不能满足开发需求。不过好在 _flutter_ 定义了非常多的动画组件，列举如下：

1.  TweenAnimationBuilder
2.  AnimatedAlign
3.  AnimatedContainer
4.  AnimatedDefaultTextStyle
5.  AnimatedScale
6.  AnimatedRotation
7.  AnimatedSlide
8.  AnimatedOpacity
9.  AnimatedPadding
10. AnimatedPhysicalModel
11. AnimatedPositioned
12. AnimatedPositionedDirectional
13. AnimatedTheme
14. AnimatedCrossFade
15. AnimatedSize
16. AnimatedSwitcher

#### 1. AnimatedContainer

 本文介绍`AnimatedContainer`，它可以看作`Container`的动画版，功能非常强大。下面例子里，点击按钮之后，组件的形状、背景、对齐方式均发生了变化。

<Image img={require('./asserts/flutter_container.gif')} alt="组件变化" /> <br />

 和透明度动画的类似，当按钮被按下时，通过`setState`方法重新渲染组件，这时组件的长度、宽度、背景颜色、对齐方式等都发生了变化，`AnimatedContainer`会在2秒中的时间完成这些属性的过渡。

    import 'package:flutter/material.dart';

    void main() {
      runApp(const Main());
    }

    class Main extends StatelessWidget {
      const Main({Key? key}) : super(key: key);

      @override
      Widget build(BuildContext context) {
        return MaterialApp(
          title: "app",
          home: Scaffold(
            appBar: AppBar(
              title: const Text("app"),
            ),
            body: const Center(child: Fade()),
          ),
        );
      }
    }

    class Fade extends StatefulWidget {
      const Fade({Key? key}) : super(key: key);

      @override
      MainState createState() => MainState();
    }

    class MainState extends State<Fade> {
      bool selected = true;
      @override
      Widget build(BuildContext context) {
        return Column(children: <Widget>[
          TextButton(
            style: ButtonStyle(
                shape: MaterialStateProperty.all<RoundedRectangleBorder>(
                    RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(18.0),
                        side: const BorderSide(color: Colors.red)))),
            onPressed: () {
              setState(() {
                selected = !selected;
              });
            },
            child: const Text('change', style: TextStyle(color: Colors.red)),
          ),
          AnimatedContainer(
            width: selected ? 200.0 : 100.0,
            height: selected ? 100.0 : 200.0,
            color: selected ? Colors.red : Colors.blue,
            alignment: selected ? Alignment.center : AlignmentDirectional.topCenter,
            duration: const Duration(seconds: 2),
            child: const FlutterLogo(size: 75),
          ),
        ]);
      }
    }

#### 2. 动画参数设置

 和`AnimatedOpacity`类似，`AnimatedContainer`也支持指定`curve`和`onEnd`回调，这里就不再赘述了，使用方法见[AnimatedOpacity](./implicity-animation.md)。

#### 3. 总结

 `AnimatedContainer`这类组件被称为 _ImplicitlyAnimatedWidget_，它们的特点是组件的属性发生变化时，动画会触发。_flutter_ 还支持显式的控制动画，这部分内容会在后面教程中介绍。

* * *

1.  [ImplicitlyAnimatedWidget class](https://api.flutter.dev/flutter/widgets/ImplicitlyAnimatedWidget-class.html)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
