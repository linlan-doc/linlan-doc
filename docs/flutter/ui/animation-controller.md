---
title: flutter动画：Animation­Controller
sidebar_position:  4
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter动画','Animation­Controller']
---

import Image from '@theme/IdealImage';

> 本文是Flutter动画系列的第三篇，建议读者先阅读前面两篇文章做到无缝衔接。

 除了动画组件，_flutter_ 还提供了灵活性更强的类：`AnimationControler`，本文介绍`AnimationControler`的使用。

### 1. flutter里的动画

 在介绍`AnimationControler`前，让我们回想一下 _flutter_ 里动画是如何实现的。假设 _ui_ 从状态 _A_ 切换到状态 _B_，首先需要有一个定时器，它每隔固定的时间会触发一次，同时有一个计算函数(_curve_)，计算 _A_ 到 _B_ 间的某个中间态。当定时器触发时，重新计算一次 _ui_ 的状态然后进行渲染，这样人眼就看到一帧一帧的动画了。

### 2. AnimationControler

 `AnimationControler`能在指定的时间里，每隔固定的时间生成[0.0,1.0]之间的数。下面代码初始化`AnimationControler`，`duration`参数指定动画的时间，`vsync`指定定时器。

    controller =
        AnimationController(duration: const Duration(seconds: 2), vsync: this);

 通过`addListener`，`AnimationControler`每次更新值时会调用回调函数，回调函数可以用来实现 _ui_ 渲染。[透明度变化动画](./implicity-animation.md)用`AnimationControler`实现如下。

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

      @override
      void initState() {
        super.initState();
        controller = AnimationController(
            duration: const Duration(milliseconds: 2000), vsync: this)
          ..addListener(() {
            setState(() {});
          });
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
          Opacity(
            opacity: controller.value,
            child: const Text("我在这"),
          )
        ]);
      }
    }

 这里需注意的点有：

1.  `controller`通过`addListener`设置回调，回调函数只是调用了`setState`方法，重新渲染 _ui_，因为透明度使用`controller.value`，所以每次重新渲染 _ui_ 透明度都会发生变化。

2.  按钮点击时，会判断`controller`的状态，如果动画已完成，则反向（即透明度从1变成0）；如果动画停在开始，则执行动画。从这也可以看出为什么叫`controller`了，有控制动画的意味。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
