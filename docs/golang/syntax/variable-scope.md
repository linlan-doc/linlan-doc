---
title: go作用域
sidebar_position:  2
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','scope','go作用域']
---
import Image from '@theme/IdealImage';

 作用域指声明的标识符在代码中的可见范围，[标识符](./basic-syntax)包括变量、结构体、方法、常量等。在 _go_ 里，作用域通过[代码块](./go-block)实现。

1.  预定义标识符全局可见

 预定义标识符包含基础数据类型、空指针、常用函数等。

    Types:
    	any bool byte comparable
    	complex64 complex128 error float32 float64
    	int int8 int16 int32 int64 rune string
    	uint uint8 uint16 uint32 uint64 uintptr

    Constants:
    	true false iota

    Zero value:
    	nil

    Functions:
    	append cap close complex copy delete imag len
    	make new panic print println real recover

2.  在顶层声明的常量、类型、变量、函数（非方法）属于包代码块。

3.  被导入的包名属于文件代码块，在导入它的文件里可见。

4.  方法接收器、函数参数等属于函数体代码块。

5.  函数内部的常量或者变量的作用域开始于声明结束，结束于最近的代码块。下面代码无法通过编译，提示找不到`f`。


    f := func() {
        f()
    }

 修改成下面的形式(先声明)，可以通过编译。

    var f func()
    f = func() {
        f()
    }

 如果在函数外部声明`f`，会提示循环引用。

6.  函数内部类型标识符的作用域开始于标识符，结束于最近的代码块。例如树节点定义，可以在定义里引用自己。


    type Node struct {
        Left, Right *Node
    }

7.  标识符重复定义

 同一个标识符不能在同一个代码块重复定义，但是可以在内部代码块定义。例如：

    package main

    import (
    	"fmt"
    )

    func main() {

    	v := "outer"
    	fmt.Printf("\nv outer :%s", v)
    	{
    		v := "inner"
    		fmt.Printf("\nv inner :%s", v)
    	}
    	fmt.Printf("\nv outer :%s", v)
    }

<Image img={require('./asserts/golang-1.png')} alt="执行结果" />

* * *

1.  [Understanding variable scope in Go](https://stackoverflow.com/questions/52503560/understanding-variable-scope-in-go)
2.  [Scopes in Go](https://medium.com/golangspec/scopes-in-go-a6042bb4298c)
3.  [go specification](https://go.dev/ref/spec#Declarations_and_scope)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
