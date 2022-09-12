---
title: flutter包管理
sidebar_position:  2
toc_max_heading_level: 4
keywords: ['flutter语言教程','flutter项目实战','flutter包管理','flutter依赖管理']
---

import Image from '@theme/IdealImage';

 项目开发过程中，一定会使用到三方包。和其他语言类似，_flutter_ 提供了包发布、下载和管理机制来支持应用使用三方包。


#### 1. 中心仓库

 _flutter_ 采用中心仓库机制，中心仓库的地址为[pub.dev](https://pub.dev/)。你可以在仓库首页搜索你需要的三方包，也可以在页面下方查看比较热门的三方包。

<Image img={require('./asserts/flutter1.png')} alt="pub首页" /><br />

 详情页除了包的介绍、使用方法之外，还有包的安装命令。

<Image img={require('./asserts/flutter2.png')} alt="flutter包安装" /><br />


#### 2. 添加依赖

 _flutter_ 项目里使用 _pubspec.yaml_ 来进行包管理。_pubspec.yaml_ 包含很多项目的描述信息，例如标题、说明、版本等。其中 _dependencies_ 表示项目依赖的三方包。

    name: flutterwhatsapp
    description: A new Flutter project.
    publish_to: 'none' # Remove this line if you wish to publish to pub.dev
    version: 1.0.0+1
    environment:
      sdk: ">=2.7.0 <3.0.0"
    dependencies:
      flutter:
        sdk: flutter
      cupertino_icons: ^0.1.3
      camera: ^0.5.8+5
      story_view: ^0.12.3
    dev_dependencies:
      flutter_test:
        sdk: flutter
    flutter:
      uses-material-design: true

 将需要引入的三方包添加到 _dependencies_ 下，执行`flutter pub get.`即可完成安装。_Windows_ 下三方包的安装路径为：`$FlutterHome\.pub-cache\hosted\pub.dartlang.org`。

#### 3. 包冲突

 如果项目依赖的包 _A_ 和 _B_ 同时依赖不同版本的 _C_，那么就出现了包冲突，解决包冲突最常用的办法是在声明依赖时不要指定具体的版本，而使用版本以上，例如：

    dependencies:
      url_launcher: ^5.4.0    # Good, any version >= 5.4.0 but < 6.0.0
      image_picker: '5.4.3'   # Not so good, only version 5.4.3 works.


#### 4. 设置镜像地址

 _flutter_ 支持修改中心仓库的地址，在执行 _flutter_ 相关命令之前，需要设置以下两个环境变量。_Windows_ 下修改 _profile_ 可以参考[PowerShell使用](/blog/ps-usage)。

    export PUB_HOSTED_URL=https://pub.flutter-io.cn
    export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn


* * *

1.  [Using packages](https://docs.flutter.dev/development/packages-and-plugins/using-packages)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
