---
title: go工作区
sidebar_position:  8
keywords: ['go编程','go工作区','go语言学习','go教程']
---

:::tip

大型项目通常包含多个模块，以经典的_MVC_架构为例，会包含 _DAO_ 、_Service_、_Controller_ 等三层。每一层包含一个或多个模块，层与层之间通过接口进行交互。分层可以确定边界，降低复杂度，提高代码的可维护性。

:::

 _go_ 在1.8版本引入工作区模式，在介绍 _go_ 工作区之前，先看一下 _maven_ 多模块的目录结构。

    root
    ├── pom.xml
    ├── dao
    │   └── pom.xml
    ├── service
    │   └── pom.xml
    └── controller
        └── pom.xml

### 1. 新建main模块

 新建工作区，进入工作区，新增 _main_ 文件夹，进入 _main_ 文件夹，初始化 _main_ 模块，_main_ 是工作空间的第一个模块，在 _main_ 模块下新建 _main.go_ 文件，作为程序的入口。关于目录、包和模块的关系，如果有疑惑，可以参考[go模块学习笔记第5节的内容](./module)。

    mkdir workspace
    cd workspace
    mkdir main
    cd main
    go mod init example.com/main

 此时工作空间的目录结构如下

    workspace
    └── main
        ├── go.mod
        └── main.go

 _main_ 里面调用 _golang.org/x/example/stringutil_ 里面的字符串翻转函数 _Reverse_，将 _Hello_ 翻转为 _olleH_。

    package main

    import (
    	"fmt"

    	"golang.org/x/example/stringutil"
    )

    func main() {
    	fmt.Println(stringutil.Reverse("Hello"))
    }

### 2. 新建工作区

 在 _workspace_ 根目录下，初始化工作区，并指定将 _main_ 模块加入到工作区。

    go work init ./main

 执行完成后，根目录下增加了 _go.work_ 文件，内容如下。此时在 _workspace_ 根目录下可以执行 _moudle main_ 的代码，`go run example.com/main`

    go 1.18

    use ./main

### 3. 增加模块

 新增一个模块，替换掉 _main_ 模块里依赖的 _golang.org/x/example/stringutil_。

 在 _workspace_ 根目录下新建文件夹 _stringutil_，进入 _stringutil_，初始化新的模块。

    go mod init example.com/stringutil

 在 _stringutil_ 下新建 _main.go_ 文件，编写 _Reverse_ 方法。

    package stringutil

    func Reverse(string) string {
    	return "abc"
    }

 _stringutil_ 模块已经创建完毕，现在将它加入到 _workspace_ 里，供 _main_ 模块使用。

    go work use ./stringutil

### 4. 调用本地stringutil模块

 将 _main_ 模块里 _golang.org/x/example/stringutil_ 替换成本地的 _stringutil_ 。修改 _main_ 模块里面的 _main.go_，执行`go run example.com/main`，可以看到替换成功。

    package main

    import (
    	"fmt"

    	"example.com/stringutil"
    )

    func main() {
    	fmt.Println(stringutil.Reverse("Hello"))
    }

 此时，_workspace_ 的目录结构如下。对比工作区目录结构和文章开头的 _maven_ 的目录结构，相似度非常的高。

    workspace
    ├── go.work
    ├── main
    │   ├── main.go
    │   ├── go.mod
    │   └── go.sum
    └── stringutil
        ├── main.go
        ├── go.mod
        └── go.sum

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
