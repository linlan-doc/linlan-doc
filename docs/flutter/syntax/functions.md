---
title: flutter基础语法：方法
sidebar_position:  9
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter方法']
---

import Image from '@theme/IdealImage';

#### 1. 定义

 作为一种面向对象语言，_Dart_ 里方法也是对象，也有类型。_Dart_ 里用`Function`表示方法类型，方法类型可以作为参数、返回值等。例如：

    Function functionGenerate(Function t) {
      return (String s) {
        return Function.apply(t, [s]) + s;
      };
    }

    String stringOri(String s) {
      return "ori" + s;
    }

    void main() {
      print(functionGenerate(stringOri)("123"));
    }

<Image img={require('./asserts/flutter16.png')} alt="运行结果" /><br />

 上面代码里，`functionGenerate`的入参是一个方法，返回值也是一个方法。其中返回值的方法的参数是`String`。代码执行流程如下：

1.  `Function.apply(t,[s])`执行`stringOri(s)`，返回`ori123`。
2.  执行`+ s`，输出`ori123123`。

#### 2. 参数

  _Dart_ 里`Function`的参数有三种，第一种是 _required positional parameters_，第二种是 _named parameters_ ，第三种是 _optional positional_ 。例如：

    int square(int width, int height) {
      return width * height;
    }

 这里的`width`和`height`属于 _required positional parameters_。_required positional parameters_ 后面可以跟 _named parameters_ 或者 _optional positional parameters_ (不能同时)。例如:

    int volumn(int length, int width, {int height = 1}) {
      return length * width * height;
    }

    void main() {
      print(volumn(4, 5));
      print(volumn(4, 5, height: 2));
    }



<Image img={require('./asserts/flutter17.png')} alt="运行结果" /><br />

 上面例子中,`{}`表示 _named parameters_，_named parameters_ 默认是可选的，当然也可以在参数声明处加上`required`，变成必选参数，此时下面代码无法通过编译。

    int volumn(int length, int width, {required int height}) {
      return length * width * height;
    }

    void main() {
      print(volumn(4, 5)); //无法通过编译
      print(volumn(4, 5, height: 2));
    }

 _optional positional parameters_ 使用`[]`标识，例如：

    int volumn(int length, int width, [int height = 1]) {
      return length * width * height;
    }

    void main() {
      print(volumn(4, 5));
      print(volumn(4, 5, 2));
    }


<Image img={require('./asserts/flutter17.png')} alt="运行结果" /><br />


 如果参数声明时没有使用`?`表示参数可为空，根据 _null safety_ 的要求，必须为参数指定默认值。例如上面的`int height=1`，下面是更多默认值示例。

    void doStuff(
        {List<int> list = const [1, 2, 3],
        Map<String, String> gifts = const {
          'first': 'paper',
          'second': 'cotton',
          'third': 'leather'
        }}) {
      print('list:  $list');
      print('gifts: $gifts');
    }

#### 3. main方法

 每一个应用必须有一个顶层的 _main_ 方法，它是这个应用的入口。 _main_ 方法的返回值为`void`，可选参数是`List<String>`。

    void main() {
      print('Hello, World!');
    }


#### 4. 匿名方法

 _Dart_ 支持匿名方法，你可以将匿名方法赋值给变量，通过变量使用匿名方法。例如：

    void main() {
      var anonymous = (String s) => s.toUpperCase();
      print(anonymous("hello flutter"));
    }


<Image img={require('./asserts/flutter18.png')} alt="运行结果" /><br />


[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
