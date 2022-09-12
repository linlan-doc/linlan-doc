---
sidebar_position:  1
title: flutter环境搭建
toc_max_heading_level: 4

keywords: ['flutter开发环境','windows','dart']
---

import Image from '@theme/IdealImage';

 [flutter](https://docs.flutter.dev/get-started/install)支持 _Windows_ , _macOS_ , _Linux_ 和 _Chrome OS_。本文以 _Windows_ 为例，介绍 _flutter_ 开发环境的搭建。

#### 1. SDK安装

 _flutter_ 依赖 _Android Studio_，前往[Android官网](https://developer.android.com/studio)下载 _Android Studio_。

 _Android Studio_ 安装完成后， 前往 _Flutter Windows SDK_ [下载地址](https://docs.flutter.dev/get-started/install/windows)下载最新的 _SDK_ 。下载完成后，解压 _SDK_ ，将`/flutter/bin`加入到系统环境变量里。

<Image img={require('./asserts/flutter2.png')} alt="下载安装flutter插件" /> <br />

 在 _Power Shell_ 运行`flutter doctor`检查是否有依赖缺失，按照指引下载对应依赖项。

<Image img={require('./asserts/flutter3.png')} alt="检查flutter依赖" /> <br />

 如果提示找不到 _cmdline-tools_ ，你可以前往 _Android Studio_ 的 _SDK Manager_ 下载 _cmdline tools_。

<Image img={require('./asserts/flutter4.png')} alt="打开SDK Manager" />

<Image img={require('./asserts/flutter5.png')} alt="下载 cmdline tools" /> <br />

:::caution

 _Windows_ 桌面应用依赖[Visual Studio](https://visualstudio.microsoft.com/downloads/)(注意不是 _Vs Code_)，如果不需要支持 _Windows_ 桌面应用，前往命令行，执行下面命令关掉 _Windows_ 桌面应用。

    flutter config --no-enable-windows-desktop

:::

#### 2. 编辑器安装

 _flutter_ 为 _Android Studio_,_Intellij_,_Vs Code_ 等主流编辑器提供了插件，仅需前往这些编辑器的插件市场下载 _flutter_ 插件即可。以 _Vs Code_ 为例，下图为 _flutter_ 插件。

<Image img={require('./asserts/flutter1.png')} alt="下载安装flutter插件" />


#### 3. 确认是否正确安装

 打开 _Vs Code_，点击：查看→命令面板→ _Flutter: New Project._

<Image img={require('./asserts/flutter6.png')} alt="新建flutter项目" /> <br />

  _flutter_ 插件会初始化一个空的项目，点击 _Vs Code_ 右下角的设备选择，点击 _F5_ ，如果程序正常启动，表示 _flutter_ 安装成功。

<Image img={require('./asserts/flutter7.png')} alt="选择运行flutter的设备" /><br />

<Image img={require('./asserts/flutter8.png')} alt="flutter启动成功" /><br />

:::tip

 如果选择的是 _Android_，并且一直卡在`Running Gradle task 'assembleDebug'...`，可以参考[这篇文章](https://stackoverflow.com/questions/59516408/flutter-app-stuck-at-running-gradle-task-assembledebug)进行解决。如果无法下载 _jar_ 包，尝试更换 _gradle_ 镜像地址，或者[搭建代理](../../set-up-site/your-site-in-one)。设置代理可以参考[Power Shell使用](/blog/ps-usage)。

:::

* * *

1.  [找不到cmdline-tools](https://stackoverflow.com/questions/68236007/i-am-getting-error-cmdline-tools-component-is-missing-after-installing-flutter)

2.  [移除windows桌面应用](https://github.com/google/flutter-desktop-embedding/issues/544)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
