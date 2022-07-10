---
title: go控制流
sidebar_position:  2
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go流程控制']
---

### 1. if-else

 `if-else`代码块语法如下：判断条件之前初始化为可选项，如果初始化语句为空，则`;`可以去掉。`Condition`的结果必须时[布尔类型](../type/go-bool)。如果`Condition`为`true`，则执行`if`分支的代码块，否则执行`else`分支的代码块。`else`后可以立即跟随一个`if-else`。

    if InitSimpleStatement; Condition {
    	// do something
    } else {
    	// do something
    }

 下面例子，随机生成一个整数，判断整数求3的余数

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

 `for`循环的语法如下，

    for InitSimpleStatement; Condition; PostSimpleStatement {
    	// do something
    }



1. [Basic Control Flows](https://go101.org/article/control-flows.html)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
