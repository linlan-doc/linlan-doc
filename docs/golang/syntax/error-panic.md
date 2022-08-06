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

 下面例子中，`divide`函数判断除数是否为0，如果为0就会返回错误给调用者。

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

<Image img={require('./asserts/golang-10.png')} alt="执行结果" /><br />


### 2. panic

 _panic_ 类似其他编程语言里的 _Exception_ ，它指程序进入某种异常状态，需要中断当前函数的执行，并将错误立刻返回给调用者，如果调用者无法处理，那么错误会层层上抛，直到程序崩溃退出。

 _go_ 提供了内建函数`panic`供代码调用，`panic`函数的定义如下，它可以接受任意类型的参数。

    func panic(interface{})


 下面例子在函数里显示的抛出了异常，最后程序终止。

    package main

    import (
    	"fmt"
    	"time"
    )

    func testA() {
    	fmt.Println("start in test A")
    	testB()
    }

    func testB() {
    	fmt.Println("start in testB")
    	testC()
    }

    func testC() {
    	fmt.Println("start in test C")
    	panic("error in c")
    }

    func main() {

    	go testA()

    	time.Sleep(3 * time.Second)
    	fmt.Printf(" main exit")
    }


<Image img={require('./asserts/golang-12.png')} alt="执行结果" /> <br />

 _go_ 没有 _throw_,_catch_ 这样的异常处理机制，但提供了类似的异常处理方式：_defer_ 和 _recover_ 。_defer_ 语句在[go defer语句使用](./go-defer)中有介绍，`recover`和`panic`类似，也是内建函数。

    func recover() interface{}


 修改上面异常退出的例子，用`recover`函数捕获`panic`，程序显示正常退出。

    package main

    import (
    	"fmt"
    	"time"
    )

    func testA() {

    	fmt.Println("start in test A")
    	testB()
    }

    func testB() {

    	defer func() {
    		if err := recover(); err != nil {
    			fmt.Printf("catch error in test B %s", err)
    		}
    	}() //匿名函数捕获异常

    	fmt.Println("start in test B")
    	testC()
    }

    func testC() {
    	fmt.Println("start in test C")
    	panic("error in c")
    }

    func main() {

    	go testA()

    	time.Sleep(3 * time.Second)
    	fmt.Printf(" main exit")
    }


<Image img={require('./asserts/golang-13.png')} alt="执行结果" /> <br />

 `recover`捕获异常有以下几点要求：

1.  必须在 _defer_ 的函数里调用`recover`方法。


    package main

    import (
    	"fmt"
    	"time"
    )

    func testA() {

    	defer recover()

    	if err := recover(); err != nil {
    		fmt.Printf("\nrecover not in defer %s", err)
    	}
    	panic("error occur")

    	fmt.Printf("unreachable") //永远不会执行
    }

    func main() {

    	go testA()

    	time.Sleep(3 * time.Second)
    	fmt.Printf(" main exit")
    }


<Image img={require('./asserts/golang-14.png')} alt="执行结果" /> <br />

2. _panic_ 和 _recover_ 必须是同一个 _goroutine_。


    package main

    import (
    	"fmt"
    	"time"
    )

    func testA() {
    	panic("error occur")
    }

    func main() {
    	defer func() {
    		if err := recover(); err != nil {
    			fmt.Println(err)
    		}
    	}()

    	go testA()

    	time.Sleep(3 * time.Second)
    	fmt.Printf(" main exit")
    }

<Image img={require('./asserts/golang-15.png')} alt="执行结果" /> <br />

:::tip

 如何在`recover`后修改返回值？试试[named return value](./basic-syntax)

:::

* * *

1.  [Error handling and Go](https://go.dev/blog/error-handling-and-go)

2.  [Goroutines, Deferred Function Calls and Panic/Recover](https://go101.org/article/control-flows-more.html)

3.  [Defer, Panic, and Recover](https://go.dev/blog/defer-panic-and-recover)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
