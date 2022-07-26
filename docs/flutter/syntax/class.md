---
title: flutter基础语法：类
sidebar_position:  11
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter基础语法','flutter语言学习','flutter类']
---

import Image from '@theme/IdealImage';

### 1. 定义

 _Dart_ 是面向对象的编程语言，在 _Dart_ 里一切都是对象，而每一个对象是某个类的实例。_Dart_ 中用关键字`class`标识类，例如：

    class Point {
      double? x; // Declare instance variable x, initially null.
      double? y; // Declare y, initially null.
      double z = 0; // Declare z, initially 0.
    }

 上面例子定义了类`Point`，它包含三个实例变量`x,y,z`，其中`x,y`可以为空，`z`不为空。对象通过`.`操作符可以访问实例变量和方法，例如：

    class Point {
      double? x, y;
      double z = 0;
      Point(this.x, this.y);

      double distance() {
        return z * z + (y ?? 0.0) * (y ?? 0.0) + (x ?? 0.0) * (x ?? 0.0);
      }
    }

    void main() {
      var p = Point(3, 4);
      print(p.x);
      print(p.y);
      p.z = 5;
      print(p.distance());
    }

<Image img={require('./asserts/flutter19.png')} alt="运行结果" /><br />

 所有的实例变量都会隐式的生成`getter`方法，非`final`或者`late final`的实例变量会隐式的生成`setter`方法。你可以通过`set`和`get`关键字，新增实例变量。例如：

    class Point {
      double? x, y;
      double z = 0;
      Point(this.x, this.y);

      double get k => z + 1;

      double distance() {
        return z * z + (y ?? 0.0) * (y ?? 0.0) + (x ?? 0.0) * (x ?? 0.0);
      }
    }

    void main() {
      var p = Point(3, 4);
      print(p.k);
    }

### 2. 构造函数

 构造函数用来构造一个类的对象，最常见的构造函数是：在类里定义一个和类名相同的方法。例如：

    class Point {
      double x = 0;
      double y = 0;

      Point(double x, double y) {
        // See initializing formal parameters for a better way
        // to initialize instance variables.
        this.x = x;
        this.y = y;
      }
    }

 _Dart_ 还提供下面方式的构造函数。

    class Point {
      final double x;
      final double y;

      // Sets the x and y instance variables
      // before the constructor body runs.
      Point(this.x, this.y);
    }

 如果类没有声明构造函数，_Dart_ 会生成一个默认的构造函数，默认构造函数没有参数，会调用父类的无参构造函数。

#### 2.1 命名构造函数

 _Dart_ 里构造函数的形式可以是 _ClassName_ 和 _ClassName.indentify_，其中后者叫命名构造函数，例如：

    class Point {
      double x = 0, y = 0;
      Point.fromJson(Map<String, double> map) {
        x = map['x']!;
        y = map['y']!;
      }

      double get len => x + y;
    }

    void main() {
      var p = Point.fromJson({"x": 2.5, "y": 3.0});
      print(p.len);
    }

<Image img={require('./asserts/flutter20.png')} alt="运行结果" /><br />

#### 2.2 初始化列表

 初始化列表允许在执行构造函数之前对变量进行初始化，例如：

    class Point {
      double x = 0, y = 0, z = 0;
      Point.fromJson(Map<String, double> map)
          : x = map['x']!,
            y = map['y']! {
        z = map['z']!;
      }

      double get len => x + y + z;
    }

    void main() {
      var p = Point.fromJson({"x": 2.5, "y": 3.0, "z": 10});
      print(p.len);
    }

<Image img={require('./asserts/flutter21.png')} alt="运行结果" /><br />

#### 2.3 调用父类构造函数

 构造函数的执行顺序如下：

1.  初始化列表。
2.  调用父类无参构造函数。
3.  执行自己的构造函数。

 如果父类没有无参构造函数，需要指定调用某个父类的构造函数，例如：

    class Shape {
      int width = 0, height = 0;
      Shape(int width, int height) {
        this.width = width;
        this.height = height;
      }
    }

    class Rectangle extends Shape {
      int w1 = 0, w2 = 0;

      Rectangle.fromJson(Map<String, int> map)
          : w1 = map['w1']!,
            super(map['width']!, map['height']!) {
        w2 = map['w2']!;
      }
    }

    void main() {
      Rectangle a = Rectangle.fromJson({"width": 1, "height": 2, "w1": 3, "w2": 4});
      print(a.width);
    }

#### 2.4 redirect构造函数

 有些构造函数直接调用其他构造函数，这被称作 _Redirecting constructors_，例如：

    class Point {
      double x, y;

      // The main constructor for this class.
      Point(this.x, this.y);

      // Delegates to the main constructor.
      Point.alongXAxis(double x) : this(x, 0);
    }

### 3. 方法

 方法是为对象提供行为的函数，前面例子中的`distance`便是方法。

#### 3.1 操作符

 操作符是特殊的方法，_Dart_ 支持的操作符列表如下：

|  &lt; |  +  | \|       | >>>  |
| :---: | :-: | -------- | ---- |
|   >   |  /  | ^        | \[]  |
| &lt;= |  ~/ | &        | \[]= |
|   >=  |  \* | &lt;&lt; | ~    |
|   -   |  %  | >>       | ==   |

 通过实现操作符来实现对象的加减乘除等。例如：

    class A {
      int x = 0, y = 0;
      A(this.x, this.y);

      A operator +(B b) => A(x + b.x, y + b.y);
    }

    class B {
      int x = 0, y = 0;
      B(this.x, this.y);
    }

    void main() {
      A ret = A(2, 3) + B(3, 4);
      print(ret.x);
    }

<Image img={require('./asserts/flutter22.png')} alt="运行结果" /><br />

:::tip

 操作符并不要求两个操作数类型一样（`A+B`合法），因为 _Dart_ 不支持重载( _overloading_ )，所以没有办法定义多个相同的`operator`。

:::

### 4. 抽象类

 _Dart_ 使用关键字`abstract`标识一个抽象类，抽象类无法被实例化。抽象类里往往会声明抽象方法，抽象方法只有方法的签名，没有实现。

    abstract class Doer {
      // Define instance variables and methods...

      void doSomething(); // Define an abstract method.
    }

    class EffectiveDoer extends Doer {
      void doSomething() {
        // Provide an implementation, so the method is not abstract here...
      }
    }


### 5. 接口

 _Dart_ 中没有 _interface_ 这样的关键字来标识接口，在 _Dart_ 中，每一个类都隐式的声明了一个接口，这个接口包含了所有的实例变量、方法等。`implements`关键字表示实现接口，例如：

    class A {
      int a = 0;
      int compute() => a * a;
    }

    class B implements A {
      @override
      int a = 0;

      @override
      int compute() {
        // TODO: implement compute
        throw UnimplementedError();
      }
    }

:::tip

 `B`实现`A`时，需要实现`A`中定义的所有方法，包括`setter`和`getter`。

:::

 `extends`和`implements`两个关键字在继承抽象类时，行为有点类似，但是这两个关键字差别很大。

1.  `extends`表示继承某一个类，它拥有父类的所有实例变量和方法等。
2.  `implements`表示实现某一个类，它只继承了类型，并且重新实现了父类的所有方法。


[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
