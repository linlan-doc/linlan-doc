---
title: go接口类型
sidebar_position:  14
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go接口类型']
---

import Image from '@theme/IdealImage';

 _go_ 接口定义了一组方法的签名（方法和函数的区别，参考[go函数类型](./func-type)），它借鉴了 _OOP_ 编程语言里 _interface_ 的思想，但有一些不同。

### 1. 接口定义

 _go_ 里接口定义的语法如下：

    type interface_name interface {

    //method

    }

 为了方便理解接口，以生活中的例子来说明：生活中常见的动物一般会跑会叫，可以定义一个动物接口，包含跑和叫两个方法签名。注意到，`interface`里面的 _method_ 并没有`func`关键字。

    type Animal interface {
    	run()
    	bark()
    }

 接口可以用来声明变量，未初始化的接口类型变量值为`nil`，类型也为`nil`。

    var animal Animal
    fmt.Println("value of animal", animal)
    fmt.Printf("type of animal %T", animal)

### 2. 实现接口

 _go_ 没有提供 _implements_ 这样的关键字来实现接口。只要某种类型包含了接口里定义的所有方法，即该类型实现了接口。下面例子定义了`Dog`和`Cat`两种类型，并且都实现了`run`和`bark`方法，这样`Dog`和`Cat`都实现了`Animal`接口，那么 _go_ 允许将`Dog`和`Cat`赋值给`Animal`。使用`Animal`调用`bark()`和`run()`方法时，实际上调用的是`Dog`或者`Cat`对应的方法，即实现了多态。

    package main

    import "fmt"

    type Animal interface {
    	run()
    	bark()
    }

    type Dog struct {
    	name string
    }

    type Cat struct {
    	name string
    }

    func (cat *Cat) run() {
    	fmt.Println(cat.name + " likes run")
    }

    func (cat *Cat) bark() {
    	fmt.Println(cat.name + " likes bark")
    }

    func (dog *Dog) run() {
    	fmt.Println(dog.name + " is runing")
    }

    func (dog *Dog) bark() {
    	fmt.Println(dog.name + " is barking")
    }

    func main() {
    	var animal Animal = &Dog{
    		name: "my dog",
    	}
    	animal.bark()
    	animal.run()

    	animal = &Cat{
    		name: "mimi",
    	}

    	animal.bark()
    	animal.run()

    }

<Image img={require('./asserts/golang-30.png')} alt="执行结果" />

### 3. 空接口

 如果一个接口没有定义任何方法，这个接口被称为空接口，空接口用`interface{}`表示。所有的类型都实现了空接口，例如：

    package main

    import "fmt"

    func emptyInterface(i interface{}) {
    	fmt.Printf("\ntype of parameter is %T", i)
    }

    func main() {
    	emptyInterface(10)
    	emptyInterface("abc")
    	emptyInterface(10.5)
    	emptyInterface(struct{ string }{"hhh"})
    }

<Image img={require('./asserts/golang-31.png')} alt="执行结果" />

### 4. 类型断言和类型转换

 前面例子中，`Dog`和`Cat`都可以赋值给`Animal`这个接口，对于一个`Animal`类型的变量，实际的类型是`Dog`还是`Cat`呢？这个时候可以使用类型断言。下面例子里，假定`i`为`T`类型，并将`T`类型的值赋给变量`t`，如果`i`的实际类型不为`T`（即断言失败），则会抛出异常。

    t := i.(T)

 类型转换提供了更加便捷的类型断言，它的语法和`switch`类似。

    switch v := i.(type) {
    case T:
        // here v has type T
    case S:
        // here v has type S
    default:
        // no match; here v has the same type as i
    }

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
