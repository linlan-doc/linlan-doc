---
title: go控制流
sidebar_position:  3
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go流程控制','if-else','for','switch-case','goto']
---

import Image from '@theme/IdealImage';

### 1. if-else

 `if-else`代码块语法如下：判断条件`Condition`前可包含初始化语句，如果初始化语句为空，则`;`可以去掉。`Condition`表达式的结果必须是[布尔类型](../type/go-bool)。如果`Condition`为`true`，则执行`if`分支的代码块，否则执行`else`分支的代码块。`else`后可立即跟随一个`if-else`语句。

    if InitSimpleStatement; Condition {
    	// do something
    } else {
    	// do something
    }

 下面例子通过`rand.Int()`随机生成一个整数`n`，判断`n`除以3的余数。

    package main

    import (
    	"fmt"
    	"math/rand"
    )

    func main() {

    	if n := rand.Int(); n%3 == 0 {
    		fmt.Printf("\n%d mod 3 is zero", n)
    	} else if n%3 == 1 {
    		fmt.Printf("\n%d mod 3 is 1", n)
    	} else {
    		fmt.Printf("\n%d mod 3 is 2", n)
    	}

    }

### 2. for循环

 `for`循环的语法如下，和`if-else`一样，`Condition`表达式的结果必须是[布尔类型](../type/go-bool)。如果`Condition`为true,则循环继续；否则循环结束。

    for InitSimpleStatement; Condition; PostSimpleStatement {
    	// do something
    }

 `InitSimpleStatement`在第一次循环之前执行一次，`Condition`每次循环都进行判断，`PostSimpleStatement`每次循环结束时执行一次。

 _go_ 没有`while`关键字，但可以通过`for`实现`while`的功能。

    n := 1
    for n < 5 {
        n *= 2
    }
    fmt.Println(n) // 8 (1*2*2*2)

    /无限循环
    sum := 0
    for {
      sum++ // repeated forever
    }
    fmt.Println(sum) // never reached

 _go_ 通过`for range`来实现其他语言`foreach`的功能，实现对 _slices_、_maps_ 等的遍历。

    strings := []string{"hello", "world"}
    for i, s := range strings {
        fmt.Println(i, s)
    }

 和其他语言一样，_go_ 提供了`break`和`continue`两个关键字来控制`for`循环。

1.  `break`跳出最近的`for`、`switch`和`select`。
2.  `continue`继续最近的`for`循环的下一次迭代。

### 3. switch-case

 `switch-case`基础语法如下，它相当于多个`if-else`语句的组合。

    switch optstatement; optexpression{
    case expression1: Statement..
    case expression2: Statement..
    ...
    default: Statement..
    }

1.  `optstatement`可选，它支持变量声明、赋值、函数调用等。在此声明的变量的作用域为`switch`代码块，详见[变量作用域](./variable-scope)。
2.  和其他语言不同，_go_ 只会运行命中的`case`，不需要显示的使用`break`。如果希望继续执行后面的`case`，需要使用`fallthrough`。


    package main

    import (
    	"fmt"
    	"math/rand"
    )

    func main() {

        switch n := rand.Intn(10) % 3; n + 1 {
        case 1:
        	fmt.Println("n is 1")
        case 2:
        	fmt.Println("n is 2")
        	fallthrough
        case 3:
        	fmt.Println("n is 3")
        }

    }

<Image img={require('./asserts/golang-2.png')} alt="执行结果" /> <br />

:::tip

`fallthrough`不能出现在最后一个分支语句里，同时只能是分支语句里的最后一条语句。

:::

### 4. goto语句

  _go_ 支持`goto`语句，`goto`语句允许无条件的跳转到函数里的某个标签。在一些场景里（例如错误处理），使用`goto`会让代码变得更清晰。

    package main

    func main() {

    	println("start")
    	goto LABEL
    	println("end")//不会执行
    LABEL:
    	println("in label")
    }

 标签有对应的作用域，作用域之外，`goto`无法跳转到该标签。下面的代码无法通过编译。

    package main

    func main() {

    	println("start")
    	goto LABEL
    	for a := 0; a < 10; a++ {
    	LABEL:
    		println("in label")
    	}
    }

 使用`goto`可以实现`for`的功能，例如：

    package main

    func main() {
    	x := 0
    NEXT:
    	if x < 5 {
    		println(x)
    		x++
    		goto NEXT

    	}
    }

 除了`goto`可以和标签搭配使用外，`break`和`continue`也可以和标签搭配使用，不过有一些限制：

1.  `break`标签必须对应包含`break`的可中断的语句（`for`或者`switch`)。
2.  `continue`标签必须对应包含`continue`的`for`语句。

* * *

1.  [Basic Control Flows](https://go101.org/article/control-flows.html)

2.  [5 basic for loop patterns](https://yourbasic.org/golang/for-loop/)

3.  [Switch Statement in Go](https://www.geeksforgeeks.org/switch-statement-in-go/)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
