---
title: go的array和slice
sidebar_position:  9
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','array','slice']
---

import Image from '@theme/IdealImage';

#### 1. array定义

  _array_ 是一种数据类型的序列，声明时必须指定长度， 且它的长度必须是 _int_ 常量，下面代码无法通过编译。

    package main

    func main() {

    	var a int32 = 7
    	var b [a]int32
    }

#### 2. array的长度

 内建函数`len`可用来计算 _array_ 的长度。_array_ 允许通过下标访问数组里面的元素。因为下标从0开始，故下标最大为`len(a)-1`。

    package main

    import "fmt"

    func main() {

    	var b [7]int32 = [7]int32{2, 43, 5, 6, 7, 8, 5}

    	fmt.Printf("第3个元素是: %d", b[3])
    }

<Image img={require('./asserts/golang-14.png')} alt="执行结果" />

#### 3. 多维数组

 _array_ 嵌套（即数组里面的元素也是数组）即为多维数组，可以将二维数组想象成棋盘（10行9列）。多维数组里每个元素都是数组，且这些数组的长度必须相等。下面例子中，内嵌数组的长度为3，可通过外层数组的下标和内层数组的下标访问多维数组里面的元素。

    package main

    import "fmt"

    func main() {

    	var a = [2][3]int{{1, 3, 5}, {2, 4, 6}}

    	var i, j int
    	for i = 0; i < 2; i++ {
    		for j = 0; j < 3; j++ {
    			fmt.Printf("%d\t", a[i][j])
    		}
    		fmt.Println()
    	}
    }

<Image img={require('./asserts/golang-15.png')} alt="执行结果" />

#### 4. 按值拷贝

 与 _Java_ 不同，_go_ 里 _array_ 是按值拷贝，即：

1.  将数组 _a_ 赋值给数组 _b_ 时，将拷贝 _a_ 的所有内容。

2.  将数组作为参数传入方法的时候，将拷贝数组所有内容。

 从下面代码的输出可以看到，数组进行了按值拷贝。

    import "fmt"

    func main() {

    	var a = [3]int32{1, 2, 3}

    	fmt.Printf("address of a int32 in main %p", &a)
    	copyArray(a)

    	var b = a
    	fmt.Printf("\naddress of b int32 in main %p", &b)

    }

    func copyArray(b [3]int32) {

    	fmt.Printf("\naddress of int32 in func %p", &b)
    }

<Image img={require('./asserts/golang-16.png')} alt="执行结果" />

#### 5. slice

 _slice_ 和 _array_ 类似，但它在执行的过程中可以动态变化。事实上，初始化 _slice_ 时会关联一个 _array_ ，_slice_ 可以看作是 _array_ 的视图，他们共享底层的数据。

 通过内建方法 _make_ 可以初始化 _slice_， _make_ 方法的签名如下, `length`表示 _slice_ 的长度，通过`len`方法可以计算得到，`capacity`表示 _slice_ 底层数组的长度，通过`cap`方法可以计算得到。

    make([]T, length, capacity)

 下面为示例

    package main

    import (
    	"fmt"
    	"reflect"
    )

    func main() {

    	var a = make([]int, 20, 30)

    	fmt.Printf("\n length : %d, capacity :%d , type: %s", len(a), cap(a), reflect.TypeOf(a))
    }

<Image img={require('./asserts/golang-17.png')} alt="执行结果" />

#### 6. slicing

  _go_ 定义了 _slicing_ 操作，基础语法为：`a[low : high]`，上述例子中，初始化长度为20，容量为30的 _slice_ 可以修改为：

    new([30]int)[0:20]

 先定义一个长度为30的数组，然后再进行 _slicing_ 操作。_slicing_ 得到一个起点为0，长度为`high - low`,容量为`capacity - low`的结果，例如：

    a := [5]int{1, 2, 3, 4, 5}
    s := a[1:4]

    s[0] == 2
    s[1] == 3
    s[2] == 4

<Image img={require('./asserts/golang-18.png')} alt="执行结果" />

 为了使用方便，`low`和`high`可以省略。当`low`省略时，默认取0；当`high`省略时，默认取被切的操作数的长度。例如：

    a[2:]  // same as a[2 : len(a)]
    a[:3]  // same as a[0 : 3]
    a[:]   // same as a[0 : len(a)]

:::tip

当 _a_ 为执行数组的指针时，`a[low,high]`相当于`(*a)[low,high]`。实际上`new([30]int32)`返回的是指针。

:::

 _slicing_ 操作也可以作用于 _slice_，得到的 _slice_ 和底层数组共享内存。例如：

    package main

    import "fmt"

    func main() {

    	var a [10]int32 = [10]int32{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    	var slice1 = a[1:5] // slice1[0] 与 a[1]对应

    	var slice2 = slice1[2:4] // slice2[0] 、slice1[2]、a[3] 对应

    	fmt.Printf("\n a[3] 的地址：%p", &a[3])
    	fmt.Printf("\n slice1[2] 的地址：%p", &slice1[2])
    	fmt.Printf("\n slice2[0] 的地址：%p", &slice2[0])
    }

<Image img={require('./asserts/golang-19.png')} alt="执行结果" />


 事实上，_slicing_ 完整的语法为：`a[low : high : max]`，相比于`a[low : high]`，`a[low : high : max]`可以将结果的容量设置为：`max - low`，但只允许省略`low`。

#### 7. slice内部结果

  _slice_ 结构体包含三部分：指向底层数组的指针；长度；容量。

<Image img={require('./asserts/golang-20.png')} alt="slice结构" />

 当执行`make([] byte,5)`时，结构如下：

<Image img={require('./asserts/golang-21.png')} alt="slice结构" />

 执行`s = s[2:4]`时，结构如下：

<Image img={require('./asserts/golang-22.png')} alt="slice结构" />

#### 8. slice动态增长

 与 _array_ 不同，_slice_ 可以动态增长。内建函数`append`可以向 _slice_ 里添加元素，当超过 _slice_ 的容量后，_slice_ 的容量会自动增长。

    package main

    import "fmt"

    func main() {

    	a := make([]int32, 1, 2) //声明一个容量为2的slice

    	fmt.Printf("\na的长度：%d,a的容量:%d\n", len(a), cap(a))

    	a = append(a, 1, 2, 3)

    	fmt.Println(a)
    	fmt.Printf("a的长度：%d,a的容量:%d", len(a), cap(a))
    }

<Image img={require('./asserts/golang-23.png')} alt="执行结果" /> 

 `append`支持将 _slice2_ 添加到 _slice1_，但需要使用`...`将 _slice2_ 展开。

    a := []string{"John", "Paul"}
    b := []string{"George", "Ringo", "Pete"}
    a = append(a, b...) // equivalent to "append(a, b[0], b[1], b[2])"
    // a == []string{"John", "Paul", "George", "Ringo", "Pete"}

* * *

1.  [Go Slices: usage and internals](https://go.dev/blog/slices-intro)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
