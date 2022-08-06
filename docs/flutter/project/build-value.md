---
title: build value使用介绍
sidebar_position:  6
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter build_value']
---

import Image from '@theme/IdealImage';

 实际开发过程中，不可避免的需要写各种各样的数据对象。为对象里面的每一个属性编写 _setter_ 和 _getter_ 方法是非常繁琐的。_Dart_ 提供了 _build_value_ 工具自动生成这些方法，让我们更专注业务本身。

#### 1.添加依赖

 _build_vlue_ 是一个三方包，和 _Java_ 里面的 _Lombok_ 有点类似，需要添加以下依赖。

    dependencies:
      built_collection: ^5.0.0
      built_value: ^8.4.0

    dev_dependencies:
      build_runner: ^1.0.0
      built_value_generator: ^8.4.0

#### 2. 配置Vs Code模板

 _build_value_ 会自动识别需要生成代码的类，这些类的格式固定。在 _Vs Code_ 里可以配置模板：文件→首选项→配置用户代码片段，搜索`dart.json`，将下面的内容拷贝进去。

    {
        "Built Value": {
            "prefix": "blt",
            "body": [
                "abstract class ${1} implements Built<${1}, ${1}Builder> {",
                "\t${0:// fields go here}",
                "",
                "\t${1}._();",
                "",
                "\tfactory ${1}([updates(${1}Builder b)]) = _$${1};",
                "}"
            ],
            "description": "Built Value Class"
        },
        "Built Value Serializable": {
            "prefix": "blts",
            "body": [
                "abstract class ${1} implements Built<${1}, ${1}Builder> {",
                "\t${0:// fields go here}",
                "",
                "\t${1}._();",
                "",
                "\tfactory ${1}([updates(${1}Builder b)]) = _$${1};",
                "",
                "\tString toJson() {",
                "\t\treturn json.encode(serializers.serializeWith(${1}.serializer, this));",
                "\t}",
                "",
                "\tstatic ${1} fromJson(String jsonString) {",
                "\t\treturn serializers.deserializeWith(${1}.serializer, json.decode(jsonString));",
                "\t}",
                "",
                "\tstatic Serializer<${1}> get serializer => _$${1/(^[A-z]{1})/${1:/downcase}/}Serializer;",
                "}"
            ],
            "description": "Serializable Built Value Class"
        },
        "Built Value Header": {
            "prefix": "blth",
            "body": [
                "library ${1};",
                "",
                "import 'dart:convert';",
                "",
                "import 'package:built_collection/built_collection.dart';",
                "import 'package:built_value/built_value.dart';",
                "import 'package:built_value/serializer.dart';",
                "",
                "part '${1}.g.dart';",
            ],
            "description": "Built Value Imports and File Header"
        },
    }

#### 3. 编写代码

 新建一个 _person.dart_ 文件，输入`blth`，插件自动补充 _build_value_ 的头。

![i18n](./asserts/flutter_blth.gif)

 输入`blt`会补充内容。

![i18n](./asserts/flutter_blt.gif)

 执行命令`flutter packages pub run build_runner build`即可完成自动生成。此时目录下会多出一个 _person.g.dart_ 文件。

<Image img={require('./asserts/flutter7.png')} alt="允许调试外部库" /><br />

* * *

1.  [Built Value Tutorial for Dart & Flutter](https://resocoder.com/2019/01/16/built-value-tutorial-for-dart-flutter/)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
