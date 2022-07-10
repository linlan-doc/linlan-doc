---
title: go函数类型
sidebar_position:  13
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go函数类型']
---

import Image from '@theme/IdealImage';

### 1. 函数

 函数是完成某种特定任务的语句集合，例如：_main_ 包里的 _main_ 函数是 _go_ 应用的入口。函数定义的语法如下：

    func function_name( [parameter list] ) [return_types] {
      body of the function
    }

-   `func` 定义函数的关键字

-   `function_name` 函数的名字

-   `parameter list` 函数接受的参数列表

-   `return_types` 函数的返回值列表

-   `body of function` 函数的内容

 通过函数名称加上函数需要的参数，就可以调用函数。调用函数时，程序的控制流转移到被调用函数，被调用函数执行完成后，执行结果返回给调用方，并归还控制流。

    package main

    import "fmt"

    func max(a, b int32) int32 {

    	if a > b {
    		return a
    	} else {
    		return b
    	}

    }

    func main() {
    	var m int32 = 20
    	var n int32 = 30

    	var maxValue = max(m, n)

    	fmt.Printf("max is %d", maxValue)
    }

### 2. 方法

 方法（_Method_）和函数非常相似，只是在函数上加上了接收者（位于`func`关键字和函数名之间）。这种设计思路来源于 _OOP_ 里对象方法。

 下面例子定义了正方形`Rec`，它有两个属性：宽和高。定义了`area`函数，它的接收者为`Rec`，这时在函数体里，可以直接使用`rec`。在调用`area`方法时，使用`rec.area()`即可。

    package main

    import "fmt"

    type Rec struct {
    	width  int
    	height int
    }

    func (rec Rec) area() int {
    	return rec.width * rec.height
    }

    func main() {
    	var rec Rec = Rec{
    		width:  10,
    		height: 5,
    	}

    	fmt.Printf("the area of rec is %d", rec.area())
    }

:::tip

_go_ 支持指针接收者，这样避免按值拷贝。即：

    func (rec *Rec) area() int {
    	return rec.width * rec.height
    }

:::

### 3. 函数类型

 函数类型是具有相同参数、相同返回值的函数的集合。下面例子，定义了参数为`string`，返回值为`string`的函数类型`FunctionType`。可以用`FunctionType`定义变量、定义函数参数等。需要注意的是`english`这个函数，在代码里面并没有显示的声明它是`FunctionType`类型的，但因为它的参数和返回值与`FunctionType`的定义一致，故它也属于`FunctionType`。

    package main

    import "fmt"

    type FunctionType func(s string) string

    func english(s string) string {
    	return "say english"
    }

    func functionTypeParam(f FunctionType) string {
    	return f("param")
    }

    func main() {
    	var f FunctionType = func(s string) string {
    		return s + "def"
    	}

    	fmt.Printf("\n f result %s", f("hello"))

    	fmt.Printf("\n function type f %s", functionTypeParam(f))
    	fmt.Printf("\n function type english %s", functionTypeParam(english))
    }

<Image img={require('./asserts/golang-29.png')} alt="执行结果" />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
