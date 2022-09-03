---
title: flutter如何实现多属性动画
sidebar_position:  5
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter动画','Simultaneous animations']
---

import Image from '@theme/IdealImage';

> 本文是Flutter动画系列的第四篇，建议读者阅读前面的教程，做到无缝衔接。

 在[第三篇](./animation-controller.md)教程里介绍了`AnimatedWidget`，这里引出一个问题：如果 _Widget_ 有多个属性需要变化，该如何实现。例如下图中，背景颜色和组件大小同时发生变化。

<Image img={require('./asserts/flutter_multi_prop.gif')} alt="多个属性同时变化" /> <br />


 `Tween`对象提供了`evaluate`方法，它的入参是`Animation`，返回当前对应的值。这样每次`Animation`生成新值时，可以通过`evaluate`方法计算`Tween`对应的值。

    evaluate(Animation<double> animation) → Color?
    The current value of this object for the given Animation. [...]

 颜色和组件同时变化实现如下，这里定义了`CurvedAnimation`来设置 _forward_ 和 _reverse_ 的 _curve_ ，然后将`CurvedAnimation`传入`AnimatedWidget`，作为`Tween`的计算入参，得到对应的属性值。

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

    class MainState extends State<Fade> with SingleTickerProviderStateMixin {
      late AnimationController controller;
      late Animation<double> curved;

      @override
      void initState() {
        super.initState();
        controller = AnimationController(
            duration: const Duration(milliseconds: 2000), vsync: this);

        curved = CurvedAnimation(parent: controller, curve: Curves.bounceOut);
      }

      @override
      void dispose() {
        controller.dispose();
        super.dispose();
      }

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
                if (controller.isDismissed) {
                  controller.forward();
                } else if (controller.isCompleted) {
                  controller.reverse();
                }
              });
            },
            child: const Text('change', style: TextStyle(color: Colors.red)),
          ),
          TextWidget(curved: curved)
        ]);
      }
    }

    class TextWidget extends AnimatedWidget {
      TextWidget({
        Key? key,
        required Animation<double> curved,
      }) : super(key: key, listenable: curved);

      final sizeTween = Tween<double>(begin: 0, end: 300);
      final colorTween = ColorTween(begin: Colors.blue, end: Colors.green);

      @override
      Widget build(BuildContext context) {
        final animation = listenable as Animation<double>;
        return Container(
          color: colorTween.evaluate(animation),
          width: sizeTween.evaluate(animation),
          height: sizeTween.evaluate(animation),
          child: const Text("我在这"),
        );
      }
    }

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
