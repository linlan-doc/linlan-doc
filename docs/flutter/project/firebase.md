---
title: Windows下安装FireBase
sidebar_position:  3
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter开发','firebase安装']
---

import Image from '@theme/IdealImage';

 [firebase](https://firebase.google.com/)是 _Google_ 公司的一款 _app_ 开发、部署、监控工具。它的 _crash_ 分析、性能分析等功能可以帮助开发者优化自己的应用。本文介绍 _Windows_ 下安装 _firebase_ 。

### 1. 安装firebase Cli

 前往[官方网站](https://firebase.google.com/docs/cli?authuser=0&hl=en#windows-standalone-binary)下载 _cli_ ，下载完成之后会得到 _exe_ 文件。打开该文件会提示你进行登录。如果无法登录，则需要使用代理。退出 _firebase cli_ ，打开 _Power Shell_ ，进入 _firebase cli_ 所在文件夹，运行 _firebase cli_ ，再进行登录。

:::tip

 搭建代理可以参考[建站攻略](../../set-up-site/your-site-in-one)，_Power Shell_ 设置代理参考[Powser Shell使用](/blog/ps-usage)。_exe_ 文件名需要修改成 _firebase_，同时将该文件路径添加到系统路径中。

:::

### 2. 激活firebase

 打开 _Power Shell_ 执行`dart pub global activate flutterfire_cli`即可激活 _firebase_ 。注意，激活程序默认将 _flutterfire_ 安装到以下路径，需要将该路径添加到系统路径中。

    C:\Users\$currentUser\AppData\Local\Pub\Cache\bin

### 3. 配置项目

 前往你的 _flutter_ 项目根目录，执行以下命令即可：

    flutterfire configure --project=your-firebase-project

:::caution

 如果执行`flutterfire configure`时提示`ERROR: The FlutterFire CLI currently requires the official Firebase CLI to also be installed`，需要将 _firebase cli.exe_ 拷贝到你的 _flutter_ 项目下。

:::

* * *

1.  [FirebaseCommandException: An error occured on the Firebase CLI when attempting to run a command](https://stackoverflow.com/questions/70410843/firebasecommandexception-an-error-occured-on-the-firebase-cli-when-attempting-t)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
