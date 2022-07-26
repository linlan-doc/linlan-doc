---
title: flutter基础语法：关键字
sidebar_position:  1
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter关键字']
---

import Image from '@theme/IdealImage';

 关键字是编程语言定义的具有特点含义和使用场景的标识符，_Dart_ 语言定义的关键字有：

| abstract <sup>2</sup> |          else          |   import<sup>2</sup>  |   show<sup>1</sup>  |
| :-------------------: | :--------------------: | :-------------------: | :-----------------: |
|     as<sup>2</sup>    |          enum          |           in          |  static<sup>2</sup> |
|         assert        |   export<sup>2</sup>   | interface<sup>2</sup> |        super        |
|   async<sup>1</sup>   |         extends        |           is          |        switch       |
|   await<sup>3</sup>   |  extension<sup>2</sup> |    late<sup>2</sup>   |   sync<sup>1</sup>  |
|         break         |  external<sup>2</sup>  |  library<sup>2</sup>  |         this        |
|          case         |   factory<sup>2</sup>  |   mixin<sup>2</sup>   |        throw        |
|         catch         |          false         |          new          |         true        |
|         class         |          final         |          null         |         try         |
|         const         |         finally        |     on<sup>1</sup>    | typedef<sup>2</sup> |
|        continue       |           for          |  operator<sup>2</sup> |         var         |
| covariant<sup>2</sup> |  Function<sup>2</sup>  |    part<sup>2</sup>   |         void        |
|        default        |     get<sup>2</sup>    |  required<sup>2</sup> |        while        |
|  deferred<sup>2</sup> |    hide<sup>1</sup>    |        rethrow        |         with        |
|           do          |           if           |         return        |  yield<sup>3</sup>  |
|  dynamic<sup>2</sup>  | implements<sup>2</sup> |    set<sup>2</sup>    |                     |

 在编码过程中避免使用关键字作为自定义标志符，但是带上标的关键字在特定的场景下可以作为自定义标识符。

-   1类关键字属于上下文关键字，这类关键字只在特定的场景下使用。这些场景之外，它们都是合法的自定义标识符。
-   2类关键字属于内建标识符，在多数场景下是合法的自定义标识符，但是不得用作类名、类型名或者导入的前缀。
-   3类关键字用于异步调用，故在标有`async, async*, or sync*.`的函数体里面，`await`或者`yield`不能作为自定义标识符使用。

 其他没有上标的关键字不可作为自定义标识符。

* * *

1.  [language-tour](https://dart.dev/guides/language/language-tour#keywords)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
