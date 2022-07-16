---
title: go代码块
sidebar_position:  4
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go blocks','go代码块']
---

 代码块是 _go_ 里非常重要的概念，它直接影响了声明标识符的可见性（标识符是否可以访问）。代码块通过`{}`声明，例子如下。

    package main

    import (
    	"fmt"
    )

    func main() {
    	{
    		var a int = 32
    		fmt.Printf("can access a %d", a) //正常访问a
    	}

    	fmt.Printf("can not access a %d", a) //无法访问a
    }

 除了显式声明，_go_ 还定义了以下隐式代码块。

1.  所有的 _go_ 源代码形成全局代码块。
2.  每一个包拥有一个包代码块，这个代码块包含该包下所有源代码。
3.  每一个文件拥有一个文件代码块，这个代码块包含该文件里所有源代码。
4.  `if-else`,`for`,`switch`都是一个代码块。
5.  `switch`,`select`的每一个条件是一个代码块。

 以`if-else`语句为例，它的分支语句定义在`{}`里，所以并不是上面提到的隐式代码块。`if-else`的隐式代码块指整个语句，下面例子初始化了变量`n`，在`if-else`分支语句里可以正常访问，在`if-else`语句之外就无法访问。

    package main

    import (
    	"fmt"
    	"math/rand"
    )

    func main() {

    	if n := rand.Int(); n%3 == 0 {
    		fmt.Printf("\n%d mod 3 is zero", n) //正常访问
    	} else if n%3 == 1 {
    		fmt.Printf("\n%d mod 3 is 1", n) //正常访问
    	} else {
    		fmt.Printf("\n%d mod 3 is 2", n) //正常
    	}

    	fmt.Printf("\n%d mod 3 is 2", n) //无法访问到n

    }

--- 

1.  [go specification](https://go.dev/ref/spec)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
