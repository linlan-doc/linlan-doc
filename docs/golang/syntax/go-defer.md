---
title: go defer语句使用
sidebar_position:  5
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go defer']
---

import Image from '@theme/IdealImage';

#### 1. 定义

 `defer`语句能够延迟执行某个函数（_deferred function_)，直到使用`defer`语句的函数返回前。例如：

    package main

    import "fmt"

    func power(i int) int {
    	fmt.Printf("\n i is %d and result is %d ", i, i*i)
    	return i * i
    }

    func main() {
    	i := 0

    	for ; i < 5; i++ {
    		defer power(i)
    	}
    	i += 5
    	power(i)

    }

<Image img={require('./asserts/golang-3.png')} alt="执行结果" /> <br />

 从上面执行的结果可以得出几个结论:

1.  _deferred function_ 的参数是在执行`defer`语句时计算的，不是在调用 _deferred function_ 时计算的（执行`defer`语句时`i`为`0,1,2,3,4`，调用 _deferred function_ 时 `i` 为10）。

2.  多个 _deferred function_ 执行时遵守 _LIFO_ （后进先出）原则。

 下面例子更清晰的说明 _deferred function_ 的参数是在执行`defer`语句的时候计算的。

    package main

    import "fmt"

    func trace(s string) string {
    	fmt.Println("entering:", s)
    	return s
    }

    func un(s string) {
    	fmt.Println("leaving:", s)
    }

    func a() {
    	defer un(trace("a"))
    	fmt.Println("in a")
    }

    func b() {
    	defer un(trace("b"))
    	fmt.Println("in b")
    	a()
    }

    func main() {
    	b()
    }

<Image img={require('./asserts/golang-4.png')} alt="执行结果" /> <br />


 `defer`能够保证函数执行完之前调用 _deferred function_(例如关闭文件)，防止执行分支过多之后出现遗漏。但同时执行的流程发生了变化（例如关闭文件在函数开始的地方），对可读性造成了一定的影响。

#### 2. panic defer recover

 `defer`关键字主要的使用场景是异常恢复，`panic defer recover`模式参考[异常处理](./error-panic)。

* * *

1.  [Defer Keyword in Golang](https://www.geeksforgeeks.org/defer-keyword-in-golang/)
2.  [Effective Go](https://go.dev/doc/effective_go#defer)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
