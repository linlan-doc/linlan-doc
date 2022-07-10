---
title: go字符串类型
sidebar_position:  6
keywords: ['go编程','go数据类型','go语言学习','go字符串类型']
toc_max_heading_level: 4
---

import Image from '@theme/IdealImage';

 _go_ 用 _string_ 来标识字符串类型，在了解字符串类型前，可以前往[基础语法](../syntax/basic-syntax)的 _String literals_ ，了解字符串常量的内容。

 字符串类型有以下特性：

1.  _string_ 底层是一个字节数组，是字符常量经过 _UTF-8_ 编码之后的结果。

2.  _string_ 是不可变更（_immutable_）的，这点和 _Java_ 一样。

3.  _string_ 类型没有方法，操作 _string_ 的方法由 _strings_ 包提供。

4.  在赋值语句中，源字符串和目标字符串共享底层的字节数组；通过 _substring_ 方法获得的子串和源字符串共享底层字节数组。

5.  空字符串可以用`""` 和 `` ` ` `` 来表示。

### 1. 字符串操作

#### 1.1 运算符

 字符串支持的运算符有以下几种

    + += == != > < >= <=

:::caution

_string_ 是不可变的，故`+、+=`不会修改源字符串，而是生成一个新字符串。

:::

 两个 _string_ 的比较，实际是底层字节数组的比较。英文字符的 _UTF-8_ 编码和 _ascii_ 一致，因此英文字符的比较按照字典的顺序。特别需要说明的是： _go_ 的`==`和 _Java_ 的区别很大，_Java_ 的`==`判断两个字符串地址是否相等，`equals` 方法才是判断字符串的内容是否相等。下面例子里，变量`a`和`b`的地址不同，但内容相同，打印`a==b`的结果为`true`。

    package main

    import "fmt"

    func main() {

    	var a string = "abc"
    	var b string = "abc"

    	printStringAddr(&a)
    	printStringAddr(&b)

    	fmt.Printf("a == b  %t", a == b)

    	var c string = "c"
    	var d string = "d"

    	fmt.Printf("\n英文字符c是否小于d  %t", c < d)

    }

    func printStringAddr(s *string) {

    	fmt.Printf("\np: %p %v\n", s, *s)
    }


<Image img={require('./asserts/golang-10.png')} alt="执行结果" />

#### 1.2 字符串长度

 通过`len`方法返回的长度是底层字节数组的长度，通过`utf8.RuneCountInString`返回的是 _rune_ 的长度。下面例子里，一共三个字符，其中英文字符占一个字节（值与 _ascii_ 一致），中文字符占三个字节。

    package main

    import (
    	"fmt"
    	"unicode/utf8"
    )

    func main() {
    	var a string = "a好的"
    	fmt.Printf("字节数组长度,%d", len(a))
    	b := []byte(a)
    	fmt.Printf("\n 字节数组为\t %v", b)

    	fmt.Printf("\n 字符长度 ：%d ", utf8.RuneCountInString(a))
    }


<Image img={require('./asserts/golang-11.png')} alt="执行结果" />

 `for range`作用于字符串时，遍历的是字符，而非字节数组。

    package main

    import (
    	"fmt"
    )

    func main() {
    	var a string = "a好的"

    	for pos, char := range a {
    		fmt.Printf("\n位置: %d , 字符: %c", pos, char)
    	}

    }


<Image img={require('./asserts/golang-13.png')} alt="执行结果" />

#### 1.3 子串

 _string_ 底层是字节数组，因此可以使用下标来访问数组里的元素。`s[i]`表示字节数组里的第`i`个字节，并不是第`i`个字符串，同时`s[i]`无法被修改。

 `s[start:end]`表示第`start`个字节（包含）到第`end`个字节（不包含），任何选取的`[start,end)`可能不是合法的 _UTF-8_ 编码。获取第`i`个字符，需要先将 _string_ 转为 _rune_ 数组，然后再取第`i`个字符。

    package main

    import (
    	"fmt"
    )

    func main() {
    	var a string = "a好的"
    	b := []byte(a)
    	fmt.Printf("\n字节数组为\t %v", b) //打印字节数组

    	fmt.Printf("\n第3个字节,%d", a[2]) //第三个字节

    	fmt.Printf("\n4 - 6 %s:", a[3:5]) //两个字节是非法的UTF-8编码

    	fmt.Printf("\n第2个字符:%s", string([]rune(a)[1])) //第二个字符好

    }


<Image img={require('./asserts/golang-12.png')} alt="执行结果" />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
