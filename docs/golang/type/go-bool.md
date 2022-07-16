---
title: go布尔类型
sidebar_position:  4
keywords: ['go编程','go数据类型','go语言学习','go布尔类型']
toc_max_heading_level: 4
---

import Spreadsheet from "react-spreadsheet";
import Image from '@theme/IdealImage';

> 数据类型是编程语言的重要概念，它决定了一类值的集合以及对应的操作。本文介绍 _go_ 的布尔类型。

 布尔类型用来表示真(true)假(false)的概念。_go_ 使用`bool`标识一个变量属于布尔类型，默认值为`false`。布尔类型常用于流程控制（常和`if`搭配使用），涉及到的运算符有以下两种。

#### 1. 比较运算符

 布尔类型可以是两个其他类型数据比较的结果，例如两个整数是否相等。_go_ 语言定义的比较运算符如下表。

<Spreadsheet data={[[{ value: ' ==' }, { value: "等于" }],[{ value: "!=" }, { value: "不等于" }],[{ value: "<" }, { value: "小于" }],[{ value: ">" }, { value: "大于" }],[{ value: "<=" }, { value: "小于等于" }],[{ value: ">=" }, { value: "大于等于" }]]} columnLabels={["运算符","含义"]} hideRowIndicators={true} />

 下面是一些比较运算符的例子，包含整数与整数、整数与浮点数（经过转换）、字符串与字符串、布尔类型与布尔类型的比较。

    import "fmt"

    func main() {

    	x := 5
    	y := 6
    	var z float32 = 6.5
    	u := true

    	s1 := "languan"
    	s2 := "doc"

    	fmt.Println("comp 1:", x < y)
    	fmt.Println("comp 2:", s1 > s2)
    	fmt.Println("comp 3:", float32(x) > z)
    	fmt.Println("comp 4:", u != (float32(x) > z))
    }


<Image img={require('./asserts/golang-9.png')} alt="执行结果" />

 两个操作数能进行比较的前提是：两个操作数能相互赋值。特殊类型的比较规则如下：

1.  同类型的指针可以进行比较（可以相互赋值），当两个指针指向同一个变量或者两个指针都为`nil`时，两者相等。

:::caution

    x=y  //将y赋值给x
    x==y //x与y是否相等

:::

#### 2. 逻辑运算符

 逻辑运算符用来计算两个或两个以上的表达式是真还是假，例如年龄超过10岁并且是女孩，它是两个条件判断（年龄超过10岁，性别为女）通过逻辑运算符（并且）连接的例子。

 _go_ 里定义了三种逻辑运算符：与(`&&`)、或(`||`)和非(`!`)。与(`&&`)和或(`||`)的真值表如下。

 `&&`真值表

<Spreadsheet data={[[{ value: 'true' }, { value: "false" }],[{ value: "false" }, { value: "false" }]]} columnLabels={["true","false"]} rowLabels={["true","false"]} />

 `||`真值表

<Spreadsheet data={[[{ value: 'true' }, { value: "true" }],[{ value: "true" }, { value: "false" }]]} columnLabels={["true","false"]} rowLabels={["true","false"]} />

 `!`运算取相反，即表达式为`true`，则非为`false`；表达式为`false`，则非为`true`。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
