---
title: flutter常用包整理
sidebar_position:  5
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter常用包']
---

import Image from '@theme/IdealImage';

#### 1. 获取app版本信息

 当你在 _app_ 里需要获取当前版本信息时，可以使用[PackageInfoPlus](https://pub.dev/packages/package_info_plus)。

    import 'package:package_info_plus/package_info_plus.dart';

    ...

    // Be sure to add this line if `PackageInfo.fromPlatform()` is called before runApp()
    WidgetsFlutterBinding.ensureInitialized();

    ...

    PackageInfo packageInfo = await PackageInfo.fromPlatform();

    String appName = packageInfo.appName;
    String packageName = packageInfo.packageName;
    String version = packageInfo.version;
    String buildNumber = packageInfo.buildNumber;

#### 2. 加载URL

 当你在 _app_ 需要打开链接时，可以使用[url_launcher](https://pub.dev/packages/url_launcher)。

    import 'package:flutter/material.dart';
    import 'package:url_launcher/url_launcher.dart';

    final Uri _url = Uri.parse('https://flutter.dev');

    void main() => runApp(
          const MaterialApp(
            home: Material(
              child: Center(
                child: ElevatedButton(
                  onPressed: _launchUrl,
                  child: Text('Show Flutter homepage'),
                ),
              ),
            ),
          ),
        );

    Future<void> _launchUrl() async {
      if (!await launchUrl(_url)) {
        throw 'Could not launch $_url';
      }
    }



[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
