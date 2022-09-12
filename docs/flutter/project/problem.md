---
title: flutter开发过程中问题汇总
sidebar_position:  4
toc_max_heading_level: 4
keywords: ['flutter语言教程','flutter项目实战','开发过程中问题']
---

import Image from '@theme/IdealImage';

 [FlutterExampleApps](https://github.com/iampawan/FlutterExampleApps)是 _github_ 上的一个开源项目，它包含很多 _flutter_ 的应用，非常适合初学者。本文记录在实践这个项目的过程中遇到的问题。

1.  Build failed due to use of deprecated Android v1 embedding.

 修改 _android\\app\\src\\main\\AndroidManifest.xml_ ，将

    <application
        android:name="io.flutter.app.FlutterApplication"
        ...

改为

    <application
            android:name="${applicationName}"
            ...

 添加下面配置

    <meta-data
            android:name="flutterEmbedding"
            android:value="2" />
              ...

2.  Type 'CheckManifest' property 'manifest' has @Input annotation used on property of type 'File'.

 找到 _android_ 文件夹下面的 _build.gradle_ ，将`com.android.tools.build:gradle`的版本改到4.2.0。

    dependencies {
            classpath 'com.android.tools.build:gradle:4.2.0'
            classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        }


3. 在Vs Code调试时提示未验证的断点。

 前往 _Dart_ 扩展的设置页面，找到允许调试 _SDK_ 和允许调试外部包这两个选项，选中即可。
<Image img={require('./asserts/flutter3.png')} alt="dart配置" /><br />

<Image img={require('./asserts/flutter4.png')} alt="允许调试sdk" /><br />

<Image img={require('./asserts/flutter5.png')} alt="允许调试外部库" /><br />

* * *

1.  ["Build failed due to use of deprecated Android v1 embedding" when building Flutter app](https://stackoverflow.com/questions/71413851/build-failed-due-to-use-of-deprecated-android-v1-embedding-when-building-flutt/71457907#71457907)

2.  [A problem was found with the configuration of task ':app:checkDebugManifest' (type 'CheckManifest') in flutter](https://stackoverflow.com/questions/67317350/a-problem-was-found-with-the-configuration-of-task-appcheckdebugmanifest-ty)

3.  [Unable to debug flutter dart code in VS Code, Unverified Breakpoint error](https://stackoverflow.com/questions/62201942/unable-to-debug-flutter-dart-code-in-vs-code-unverified-breakpoint-error)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
