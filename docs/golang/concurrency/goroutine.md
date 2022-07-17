---
title: goroutine使用
sidebar_position:  1
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','goroutine','go concurrency']
---

import Image from '@theme/IdealImage';

 _goroutine_ 是 _go_ 提供的轻量级并发编程工具，使用起来非常简单，在函数调用之前加上关键字`go`即可。例如：

    package main

    import (
    	"fmt"
    	"time"
    )

    func printMessage(from string) {
    	fmt.Println("print message from " + from)
    }

    func main() {

    	go printMessage("goroutine")
    	printMessage("main")

    	time.Sleep(1000 * time.Millisecond)
    }

<Image img={require('./asserts/go-lang1.png')} alt="执行结果" />

 `main`方法里面调用了`time.Sleep`，这是因为 _goroutine_ 需要调度， 如果 _main_ 方法退出的时候 _goroutine_ 还没有执行完成，就不会打印消息。

 在并发编程的场景中，经常需要协同。例如：`main`方法等待所有的 _goroutine_ 执行完成。_go_ 里实现协同的方式主要有以下两种：

1.  使用管道(_channel_)
2.  使用`sync.WaitGroup`

 管道(_channel_)是 _go_ 中非常重要的数据类型，它允许向管道发送消息和从管道接受消息。`<-`操作符用来发送或者接受消息，箭头方向表示数据流向。

    ch <- v    // 向管道发送消息

    v := <-ch  // 从管道获取消息，同时将值赋给v

 管道(_channel_)的特点是：如果对方没有准备好，发送或者接受将会阻塞。利用这个特性可以实现 _main_ 方法等待 _goroutine_ 执行完成。

    package main

    import (
    	"fmt"
    	"time"
    )

    func printMessage(from string, done chan bool) {
    	fmt.Println("print message from " + from)
    	time.Sleep(time.Second)
    	done <- true
    }

    func main() {

    	channel := make(chan bool)
    	go printMessage("goroutine", channel)
    	done := <-channel
    	fmt.Printf("\ngorountine finish %t\n", done)
    	fmt.Println("exit main")

    }

<Image img={require('./asserts/go-lang2.png')} alt="执行结果" />

 在`main`方法里等待`channel`里的消息，而消息会从 _goroutine_ 发送。需要说明的是，上面代码里的`channel`变量是 _unbuffered channel_ ，向 _unbuffered channel_ 发送消息时会阻塞直至消息被接受，修改上面的代码即可验证发送消息是否阻塞。

    package main

    import (
    	"fmt"
    	"time"
    )

    func printMessage(from string, done chan bool) {
    	fmt.Println("print message from " + from)
    	done <- true
    	fmt.Println("send message success " + from)//main方法调用了休眠，故这条打印记录也等待了1s
    }

    func main() {

    	channel := make(chan bool)
    	go printMessage("goroutine", channel)
    	time.Sleep(time.Second)
    	done := <-channel
    	fmt.Printf("\ngorountine finish %t\n", done)
    	fmt.Println("exit main")

    }

 与 _unbuffered channel_ 对应的是 _buffered channel_，在调用`make`方法时，指定容量得到的结果便是 _buffered channel_。_buffered channel_ 的特点是：当管道里未消费的消息超过容量时，发送消息才会会阻塞。

    c := make(chan string, 2)

 `WaitGroup`用来等待一组 _goroutine_ 完成，使用`WaitGroup`分为这几步。

1.  新建一个`WaitGroup`
2.  调用`WaitGroup`的`Add`方法，参数为需要等待的 _goroutine_ 数。
3.  在每一个 _goroutine_ 里调用`WaitGroup`的`Done`方法（相当于减一）
4.  在需要等待其他 _goroutine_ 完成的 _goroutine_ 里调用 `WaitGroup`的`Wait`方法，这样在其他 _goroutine_ 完成之前，`Wait`方法会阻塞。


    package main

    import (
    	"fmt"
    	"sync"
    )

    func main() {

    	var wg sync.WaitGroup

    	wg.Add(3)

    	for i := 0; i < 3; i++ {
    		go func() {
    			defer wg.Done()
    			fmt.Println("go routine")
    		}()
    	}
    	wg.Wait()
    	fmt.Printf("\n goroutine finished")
    }

<Image img={require('./asserts/go-lang3.png')} alt="执行结果" />

* * *

1.  [How to Wait for All Goroutines to Finish Executing Before Continuing](https://nathanleclaire.com/blog/2014/02/15/how-to-wait-for-all-goroutines-to-finish-executing-before-continuing/)

2.  [Channels](https://go.dev/tour/concurrency/2)

3.  [Go: Buffered and Unbuffered Channels](https://medium.com/a-journey-with-go/go-buffered-and-unbuffered-channels-29a107c00268)

4.  [Go by Example: Goroutines](https://gobyexample.com/goroutines)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
