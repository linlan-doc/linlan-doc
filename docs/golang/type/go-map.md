---
title: go map类型
sidebar_position:  15
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go map类型']
---

import Image from '@theme/IdealImage';

>  _hash table_ 是计算机科学中非常重要的数据结构，它的特性有很多，其中快速查询是最常使用的特性。_go_ 提供了 _map_ 类型，它底层是 _hash table_。

### 1. 定义

 _map_ 定义形式如下，它分为键(`KeyType`)和值(`ValueType`)，下面例子中`m`的`KeyType`为`string`,`ValueType`为`int`。

    map[KeyType]ValueType
    var m map[string]int

 _map_ 常用的使用场景是获取 _key_ 对应的值，这就需要判断 _map_ 里面的 _key_ 与用户指定的 _key_ 是否相等，所以 _map_ 要求 `KeyType` 是[可比较](./go-bool)的。`slice`、`map`和函数无法作为 _key_ 。

    m["route"] = 66 //将route这个key对应的值设置为66

    i := m["route"] //获取route这个key对应的值，并将他赋给i

### 2. 常用操作

#### 2.1 初始化

 仅声明 _map_ 不初始化，_map_ 的值为`nil`。当读 _nil map_ 时和读一个空 _map_ 一样，但是写 _nil map_ 时，会抛出异常。

    package main

    import (
    	"fmt"
    )

    func main() {

    	var m map[string]int
    	t, ok := m["abc"]

    	fmt.Printf("\n t is %d, ok is %t", t, ok)

    	m["def"] = 1
    }

<Image img={require('./asserts/golang-32.png')} alt="执行结果" />

:::tip

 当 _key_ 对应的 _value_ 不存在时，_map_ 会返回值类型对应的0值。上面例子里，`int`类型的0值为0。

:::

 _map_ 的初始化方式有两种：1. 调用内建函数 `make`。2. 使用 _map literal_ 。

    package main

    func main() {

    	var a map[string]int = make(map[string]int)
    	var b map[string]int = map[string]int{
    		"abc": 1,
    		"def": 2,
    	}

    }

#### 2.2 函数调用

 内建函数`len`可以返回 _map_ 里面的条目数；`delete`方法可以删除 _map_ 里的某一个 _key_；判断 _key_ 是否存在可以使用二目赋值符；`for-range`可以遍历 _map_ 里的键值对。

    n := len(m) //获取map里的条目数

    delete(m, "route") //删除route对应的值

    i, ok := m["route"] //i表示route对应的值，ok表示route是否存在。

    for key, value := range m {
      fmt.Println("Key:", key, "Value:", value)
    }
    //遍历m

#### 2.3 值为空接口

 [空接口](./go-interface)类型用`interface{}`表示，所有类型都实现了空接口。如果 _map_ 的值类型为`interface{}`，意味着值可以是任意类型。

    package main

    import (
    	"fmt"
    )

    func main() {

    	type FuncType func(s string) string

    	var c FuncType = func(s string) string {
    		return s + "funType"
    	}

    	d := func(s string) string {
    		return s + "sfunc"
    	}

    	var a map[string]interface{} = map[string]interface{}{
    		"a": 1,
    		"b": "kkk",
    		"c": c,
    		"d": d,
    	}

    	for k, v := range a {
    		switch vType := v.(type) {
    		case int:
    			fmt.Printf("\nkey is %s and value is int %d", k, vType)
    		case string:
    			fmt.Printf("\nkey is %s and value is int %s", k, vType)
    		case FuncType:
    			fmt.Printf("\nkey is %s and value is int %s", k, vType(k))
    		case func(s string) string:
    			fmt.Printf("\nkey is %s and value is int %s", k, vType(k))
    		}
    	}

    }


<Image img={require('./asserts/golang-33.png')} alt="执行结果" />


#### 2.4 并发访问

 _map_ 不是并发安全的，如果在并发的 _goroutines_ 里访问 _map_ 需要进行加锁。例如：

    var counter = struct{
        sync.RWMutex
        m map[string]int
    }{m: make(map[string]int)}

    counter.RLock() //promoted fields
    n := counter.m["some_key"]
    counter.RUnlock() //promoted fields
    fmt.Println("some_key:", n)

:::tip

 关于 _promoted fields_ 可以参考[go 结构体](./go-struct)

:::

* * *

1.  [Go maps in action](https://go.dev/blog/maps)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
