---
title: go数值类型
sidebar_position:  5
keywords: ['go编程','go数据类型','go语言学习','go数值类型']
toc_max_heading_level: 4
---

<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.13.24/dist/katex.min.css" integrity="sha384-odtC+0UGzzFL/6PNoE8rX/SPcQDXBJ+uRepguP4QkPCm2LBxH3FA3y+fKSiJ+AmM" crossorigin="anonymous" />
</head>

 _go_ 定义了三种数值类型：整型、浮点型和复数型（$i^2=-1$），架构无关的数值类型有：

    uint8       8位无符号整数 (0 to 255)
    uint16      16位无符号整数 (0 to 65535)
    uint32      32位无符号整数 (0 to 4294967295)
    uint64      64位无符号整数 (0 to 18446744073709551615)

    int8        8位有符号整数 (-128 to 127)
    int16       16位有符号整数 (-32768 to 32767)
    int32       32位有符号整数 (-2147483648 to 2147483647)
    int64       64位有符号整数 (-9223372036854775808 to 9223372036854775807)

    float32     IEEE-754 32位浮点数
    float64     IEEE-754 64位浮点数

    complex64   32位实数+32位虚数
    complex128  64位实数+64位虚数

    byte        uint8的别名
    rune        int32的别名

 和实现有关的数值类型有：

    uint     32或者64
    int      同uint
    uintptr  an unsigned integer large enough to store the uninterpreted bits of a pointer value

 提到数值类型，就不得不提隐式转换。_go_ 语言的设计者认为隐式转换带来的便利远不及它带来的困扰，所以 _go_ 不支持对非常量进行隐式转换，看下面一个例子。
可以将10赋值给`f1`，但不允许将`f3`赋值给`f2`，因为它们的类型不同。

    package main

    func main() {

    	var f1 float32 = 10
    	var f2 float64
    	var f3 int32 = 10
    	f2 = f3
    }

 数值类型常见的问题还有溢出。上面表格标注了不同数值类型的上下限，当超出上下限时，就发生了溢出。下面例子里，`uint8`最大值为255，直接声明`c=256`无法通过编译，但是声明`b=a+1`可以通过编译，并且`b`的值为`0`，即发生了溢出。

    package main

    import "fmt"

    func main() {

    	var a uint8 = 255
    	var b uint8 = a + 1

    	var c uint8 = 255 + 1

    	fmt.Printf("计算结果为 %d", b)

    }

 _go_ 官方没有`BigDecimal`，使用比较多的是[shopspring/decimal](https://github.com/shopspring/decimal)。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
