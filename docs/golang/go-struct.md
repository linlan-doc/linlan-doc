---
title: go结构体
sidebar_position:  11
toc_max_heading_level: 4

keywords: ['go语言教程','go基础语法','go语言学习','go结构体','go struct']
---

import Image from '@theme/IdealImage';

 和其他面向对象的语言不同，_go_ 并没有 _class_ 的概念。在处理复杂类型时，_go_ 使用结构体 _struct_ 。

#### 1. struct定义

 _struct_ 是一组属性的集合，定义语法如下：

    type MyStruct struct {
       firstName string
       lastName  string
       age       int
    }

 这个 _struct_ 名为 _MyStruct_，它拥有三个属性，分别为 _string_ 类型的 _firstName_，_string_ 类型的 _lastName_ 和 _int_ 类型的 _age_。

 用结构体声明变量时，默认结构体里面的每一个属性会被赋予该属性对应类型的零值（_string_ 对应空字符串，数值型对应0）。也可以在声明变量时对结构体的属性进行初始化。

    package main

    import "fmt"

    type MyStruct struct {
    	firstName string
    	lastName  string
    	age       int32
    }

    func main() {

    	var a MyStruct
    	fmt.Printf("firstName : %s, lastName : %s , age : %d", a.firstName, a.lastName, a.age)

    	var b MyStruct = MyStruct{
    		firstName: "lin",
    		lastName:  "lan",
    		age:       12,
    	}

    	fmt.Printf("\nfirstName : %s, lastName : %s , age : %d", b.firstName, b.lastName, b.age)

    }

<Image img={require('./asserts/golang-24.png')} alt="执行结果" />

#### 2. 匿名属性

 _struct_ 允许匿名属性，属性类型即为属性的名字，因为 _struct_ 属性名字不能重复，故相同类型的匿名属性最多只能一个。

    package main

    import "fmt"

    type MyStruct struct {
    	string
    	int
    }

    func main() {

    	var b MyStruct = MyStruct{
    		string: "lin", //属性类型作为属性名称
    		int:    12,
    	}

    	fmt.Printf("\nstring : %s, int : %d", b.string, b.int)

    }

<Image img={require('./asserts/golang-25.png')} alt="执行结果" />

#### 3. 匿名结构体

 _go_ 支持匿名结构体，和 _Java_ 匿名类类似，例如：

    package main

    import (  
        "fmt"
    )

    func main() {  
        emp3 := struct {
            firstName string
            lastName  string
            age       int
            salary    int
        }{
            firstName: "Andreah",
            lastName:  "Nikola",
            age:       31,
            salary:    5000,
        } //没有type structName 这样的定义

        fmt.Println("Employee 3", emp3)
    }

#### 4. 指向结构体的指针

 对结构体执行取地址`&`操作，可以获得指向结构体的指针，通过这个指针可以访问结构体的属性。

    package main

    import (
    	"fmt"
    	"reflect"
    )

    type MyStruct struct {
    	first  string
    	second int
    }

    func main() {

    	var b = &MyStruct{
    		first:  "lin",
    		second: 12,
    	}

    	fmt.Printf("\ntype of b is %s", reflect.TypeOf(b))
    	fmt.Printf("\nstring : %s, int : %d", b.first, b.second)

    }

<Image img={require('./asserts/golang-26.png')} alt="执行结果" />

:::tip

_go_ 支持直接使用指针访问元素，`b.first`等同于`(*b).first`

:::

#### 5. 结构体嵌套

  _struct_ 的属性可以为另一个 _struct_。例如：一个学生拥有一个地址，而地址是一个 _struct_，它包含省、市、区等信息。

    package main

    import "fmt"

    type Address struct {
    	province string
    	city     string
    	area     string
    }

    type Student struct {
    	Address
    	age int32
    }

    func main() {

    	var student Student = Student{
    		Address: Address{
    			province: "河北",
    			city:     "石家庄",
    			area:     "杨浦区",
    		},
    		age: 16,
    	}

    	fmt.Printf("\nstudent age %d, his area is %s", student.age, student.area)
    }

#### 6. Promoted Field

 _struct_ 支持匿名属性，当这个属性为结构体时，_struct_ 可以直接访问这个内嵌结构体里面的属性，这被称为 _Promoted Field_ 。上面例子里,`student.area`即为 _Promoted Field_。

1.  [Structures in Go](https://medium.com/rungo/structures-in-go-76377cc106a2)
2.  [Structs](https://golangbot.com/structs/)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
