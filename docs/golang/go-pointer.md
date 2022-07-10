---
title: go指针类型
sidebar_position:  12
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go指针类型','go pointer']
---

import Image from '@theme/IdealImage';

 _go_ 指针类型存放的是某个变量的内存地址。在 _go_ 语言里，通过`*T`声明指向类型为`T`的指针。指针类型的零值为`nil`。

    package main

    import "fmt"

    func main() {

    	var p *int32     //定义一个指向 int32的指针
    	var a int32 = 10 //定义一个 int32类型的变量
    	p = &a           //将变量的地址赋给p

    	fmt.Printf("修改之前, *p : %d, a: %d", *p, a)

    	*p = 100 //修改p地址对应的值

    	fmt.Printf("\n修改之后, *p : %d, a: %d", *p, a)

    }

<Image img={require('./asserts/golang-27.png')} alt="执行结果" />

 `*`在声明变量时，表示变量是一个指针类型；当作为操作符作用于指针变量时，表示取指针指向地址的值。`*p=100`表示将指针`p`指向地址的值赋为100，`p`指向变量`a`，可以看到变量`a`的值也变成了100.

 _go_ [内建方法](https://pkg.go.dev/builtin)_new_ 能够分配内存，并且返回一个指针。该方法的签名如下：

    func new(Type) *Type

> The new built-in function allocates memory. The first argument is a type, not a value, and the value returned is a pointer to a newly allocated zero value of that type.

 当变量作为参数传入到 _func_ 时，涉及到按值传递还是按指针传递。例如：

    package main

    import "fmt"

    func passByValue(a int32) {
    	a = a + 10
    }

    func passByPointer(a *int32) {
    	(*a) = (*a) + 10
    }

    func changePointer(a *int32) {
    	var b int32 = 100
    	a = &b
    }

    func main() {
    	var a int32 = 10

    	fmt.Printf("\n初始值：%d", a)
    	passByValue(a)
    	fmt.Printf("\n调用passByValue后：%d", a)
    	passByPointer(&a)
    	fmt.Printf("\n调用passByPointer后：%d", a)
    	changePointer(&a)
    	fmt.Printf("\n调用changePointer后：%d", a)
    }

<Image img={require('./asserts/golang-28.png')} alt="执行结果" />

 `passByValue`方法里面的变量`a`和`main`方法里面的`a`是两个变量，修改`passByValue`方法里面的`a`不会影响`main`方法里面的`a`；

 `passByPointer`里面的变量`a`是指向`main`方法里`a`的指针，所以可以修改`main`方法里面`a`的值；

 `changePointer`方法里，将`b`的地址重新赋给`a`，所以不影响`main`方法里面的`a`。

 按值传递的优点是，方法对值的修改不会影响调用方，缺点是当变量是复杂类型（如 _struct_）时，拷贝值的成本比较高。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
