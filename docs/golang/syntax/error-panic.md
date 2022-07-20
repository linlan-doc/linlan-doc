---
title: 错误和异常处理
sidebar_position:  6
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','error','panic','异常恢复']
---

import Image from '@theme/IdealImage';

 程序在运行过程中，难免会出现错误。例如除数为0，想要打开的文件不存在等。_go_ 中处理异常情况的方式有两种：_error_ 和 _panic_ ，本文先介绍这两种方式的用法，最后比较两者的使用场景。

### 1. error

 在[buildin](https://pkg.go.dev/builtin#error)包可以找到`error`的定义，它是接口类型，只有一个`Error`方法，返回一个`string`。

    type error interface {
    	Error() string
    }

 很多系统函数的返回值包含`error`类型，例如`os`包里面的`Open`方法。

    func Open(name string) (*File, error)

 下面代码尝试打开一个并不存在的文件，观察`Open`方法返回的错误信息。

    package main

    import (
    	"log"
    	"os"
    )

    func main() {

    	_, err := os.Open("abc.tex")

    	if err != nil {
    		log.Fatal(err)
    	}
    }

<Image img={require('./asserts/golang-9.png')} alt="执行结果" />  <br/>

  _errors_ 包提供了 _error_ 的操作函数，下面结合[源码](https://cs.opensource.google/go/go/+/refs/tags/go1.18.4:src/errors/errors.go)介绍如何新建 _error_ 。_errors_ 包里定义了`errerString`这个结构体，它实现了`error`接口（接口实现参考[接口类型](../type/go-interface)）。`New`方法返回`errorString`。

    type errorString struct {
        s string
    }

    func (e *errorString) Error() string {
        return e.s
    }

    func New(text string) error {
        return &errorString{text}
    }

 下面例子中，`divide`函数判断除数为0，就会返回错误给调用者。

    package main

    import (
    	"errors"
    	"fmt"
    )

    func divide(a, b int) (int, error) {

    	if b == 0 {
    		return 0, errors.New("除数不能为0")
    	}

    	ret := a / b
    	return ret, nil
    }
    func main() {

    	ret, err := divide(5, 0)

    	if err != nil {
    		fmt.Println(err.Error())
    	} else {
    		println(ret)
    	}

    }

<Image img={require('./asserts/golang-11.png')} alt="执行结果" /> <br />

 用户可以自定义不同类型的 _error_，通过[类型断言](../type/go-interface)来判断不同的 _error_ 进而处理不同的逻辑。例如：

    package main

    import (
    	"fmt"
    	"math/rand"
    )

    type error1 struct{}

    type error2 struct{}

    type error3 struct{}

    func (e *error1) Error() string {
    	return "error1"
    }

    func (e *error2) Error() string {
    	return "error2"
    }

    func (e *error3) Error() string {
    	return "error3"
    }

    func test() (int, error) {
    	switch c := rand.Intn(100); c % 3 {
    	case 0:
    		return c, &error1{}
    	case 1:
    		return c, &error2{}
    	case 2:
    		return c, &error3{}
    	}
    	return 0, nil
    }

    func main() {

    	for i := 0; i < 10; i++ {
    		_, err := test()
    		switch v := err.(type) {
    		case *error1:
    			fmt.Printf("\nerror1 %s", v)
    		case *error2:
    			fmt.Printf("\nerror2 %s", v)
    		case *error3:
    			fmt.Printf("\nerror3 %s", v)
    		}
    	}

    }

<Image img={require('./asserts/golang-10.png')} alt="执行结果" />


### 2. panic

 _panic_ 类似其他编程语言里的 _Exception_ ，它指程序进入某种异常状态，需要中断当前函数的执行，并将错误立刻返回给调用者，如果调用者无法处理，那么错误会层层上抛直到 _goroutine_ 终止。

 _go_ 提供了内建函数`panic`供代码调用，`panic`函数的定义如下，它可以接受任意类型的参数。

    func panic(interface{})


* * *

1.  [Error handling and Go](https://go.dev/blog/error-handling-and-go)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
