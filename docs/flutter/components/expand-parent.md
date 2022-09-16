---
title: flutter如何和父组件一样宽
sidebar_position:  7
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter组件设计']
---

import Image from '@theme/IdealImage';

 在 _flutter_ 里，可以使用下面几种方式来让子组件和父组件宽度一样。

#### 1. SizedBox.expand

 `SizedBox.expand`是`SizedBox`的构造函数，官方的说明如下:

> Creates a box that will become as large as its parent allows.

 方法的实现如下，由此不难看出，如果只希望宽或者高和父组件一样，`expand`并不适用。

    const SizedBox.expand({ Key? key, Widget? child })
      : width = double.infinity,
        height = double.infinity,
        super(key: key, child: child);

#### 2. SizedBox手动指定宽度

 除了使用`SizedBox.expand`，也可以在构造函数里指定`width`。

    SizedBox(
      width: double.infinity,
      child: Column(...),
    )

#### 3. 使用ConstrainedBox指定最小宽度

    ConstrainedBox(
        constraints: const BoxConstraints(minWidth: double.infinity),
        child: Column(...),
    )

#### 4. 使用Container

 除了宽和高，还需要指定其他属性时，`SizedBox`无法满足需求，这种情况可以使用`Container`。

    Container(
      width: double.infinity,
      // height: double.infinity,
      child: Column(...),
    )

* * *

1.  [How to expand to Parent Widget width?](https://medium.com/flutterworld/how-to-use-match-parent-width-2e8cfe0486d6)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
