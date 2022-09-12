---
title: flutter安装应用到Android手机
sidebar_position:  9
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter应用安装','Android']
---

import Image from '@theme/IdealImage';

 在使用 _Flutter_ 开发的过程中，可以在真机上体验我们的应用。现在以安卓为例，介绍怎么将应用安装到真机上。

### 1. 连接手机并打开开发者模式

 将手机连接到 _pc_ 并在手机的设置里搜索“USB调试模式”，将该选项打开。

### 2. 安装应用

 进入应用的目录，执行`flutter install`，按照提示选择设备进行安装。

<Image img={require('./asserts/flutter8.png')} alt="选择设备" /><br />

### 3. 错误汇总

#### 3.1 INSTALL_FAILED_NO_MATCHING_ABIS

  笔者执行以下命令之后成功安装。如果下面命令没有解决问题，可以参考引用里的1和2。

    flutter clean
    flutter build apk
    flutter install

* * *

1.  [Failure \[INSTALL_FAILED_NO_MATCHING_ABIS: Failed to extract native libraries, res=-113\] Install failed](https://github.com/flutter/flutter/issues/31579)

2.  [\[INSTALL_FAILED_NO_MATCHING_ABIS: Failed to extract native libraries, res=-113\]](https://stackoverflow.com/questions/36414219/install-failed-no-matching-abis-failed-to-extract-native-libraries-res-113/57848743#57848743)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
