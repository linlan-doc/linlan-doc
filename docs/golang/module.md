---
title: go模块
sidebar_position:  7
keywords: ['go编程','go模块学习','go语言学习','go教程']
---

import Image from '@theme/IdealImage';

 模块是 _go_ 代码包的集合，模块根目录下的 _go.mod_ 文件定义了模块的路径、依赖等信息。

    module hello-world

    go 1.17

    require (
    	golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c // indirect
    	rsc.io/quote/v4 v4.0.1 // indirect
    	rsc.io/sampler v1.3.0 // indirect
    )

### 1. 创建新的模块

 在[golang开发环境搭建](/docs/golang/set-up)一文中，介绍了如何创建一个新的模块，这里就不再赘述了。

### 2. 添加依赖

 在 _golang_ 开发过程中，需要用到其他人开发的模块。像 _Maven_、_NPM_ 这些包管理工具，都有中心仓库，开发者开发好软件包之后，提交到中心仓库，其他人就可以使用了。

 _golang_ 的包管理思路不同于以上，_golang_ 采用半中心化的思路。任意的_git_仓库都可以作为 _golang_ 的包仓库，只要仔细观察你就会发现不同的包会来自不同的地址（这点和 _Maven_、_NPM_ 非常不同）。

> By default, the go command may download modules from <https://proxy.golang.org>. It may authenticate modules using the checksum database at <https://sum.golang.org>. Both services are operated by the Go team at Google.The privacy policies for these services are available at <https://proxy.golang.org/privacy> and <https://sum.golang.org/privacy>, respectively.
>
> The go command's download behavior may be configured using GOPROXY, GOSUMDB,GOPRIVATE, and other environment variables

 当执行 _go get_ 命令的时候，_golang_ 首先检查本地是否有需要安装的包，如果没有找到，就会去服务器下载。下载地址取决于 _GOPROXY_ 这个变量值，_GOPROXY_ 默认值为：_<https://proxy.golang.org>, direct_。这个默认值的含义是，如果在 _<https://proxy.golang.org>_ 没有找到需要安装的包，就去包名对应 _URL_ 下载。

 _proxy.golang.org_ 是由 _google_ 运营的模块镜像仓库，它会从源服务器同步模块到 _proxy.golang.org_ ，当用户安装时，优先从 _proxy.golang.org_ 下载。

:::caution

中国大陆屏蔽了 _proxy.golang.org_ ，只能使用其他的镜像仓库，例如：[goproxy.cn](https://github.com/goproxy/goproxy.cn/blob/master/README.zh-CN.md)。使用的时候，修改 _GOPROXY_ 的值即可。

    $ go env -w GO111MODULE=on
    $ go env -w GOPROXY=https://goproxy.cn,direct

:::

 接下来，看看执行 _go get rsc.io/quote/v4_ 后，发生了什么。

1.  _go.mod_ 文件里面多了 _require_ 的内容。
2.  多了文件 _go.sum_

 _require_ 比较好理解，在执行 _go get_ 的时候，将包的依赖加到 _go.mod_ 中。_go.sum_ 文件的每一行数据包括包名、包版本和对应的 _hash_ 值，它用来保证之后下载的包与第一次下载的包一致，防止包被恶意篡改。

 在代码里面引入 _quote_，成功调用 _quota.Hello()_ 方法。



<Image img={require('./asserts/golang-4.png')} alt="引入quote" />

:::tip

 如果无法下载 _quote_ 包，清参考[golang环境搭建](/docs/golang/set-up)里面的代理设置。

:::

### 3. 升级包依赖

 _golang_ 的包版本号的格式类似于 _v0.1.2_，其中 _0_ 是大版本号，_1_ 是小版本号，_2_ 为补丁。执行 _go list -m all_ 时，发现 _golang.org/x/text_ 使用的是没有 _tag_ 的版本，需要升级到最新的 _tag_ 版本。你会发现 _go.mod_ 里，_require_ 部分相应的发生了变化，同时 _go.sum_ 里新增了最新版的记录。

    go get golang.org/x/text



<Image img={require('./asserts/golang-5.png')} alt="升级包" />

 细心的小伙伴可能发现，包后面有 _indirect_ 的标注，这个 _indirect_ 表示不直接依赖这个包。例如 _A_ → _B_, _B_ → _C_，那么 _A_ 就间接依赖 _C_。但是上述代码里面直接引用了 _quote_ 包，为什么它也是 _indirect_？这可能是先 _go get_，后在代码里面引用的缘故，执行 _go mod tidy_ 即可。


<Image img={require('./asserts/golang-6.png')} alt="执行 go mod tidy" />

### 4. 发布包

 _go_ 支持用户发布自己开发的模块。前面提到，_Go Proxy_ 并不是一个中心化的仓库，它会同步用户发布的模块，所以需要用户先将模块发布到互联网上。最方便的发布方式是使用 _github_。不清楚如何使用 _github_ 的可以参考[github使用指南](../../blog/github-usage)。

#### 4.1 新建代码仓库

 在 _github_ 上创建一个空的仓库，将空仓库拉到本地。

#### 4.2 编写包代码

 新建模块，其中 _linlan-doc_ 是你的账号名，_golang-repo_ 是你的仓库名

    go mod init github.com/linlan-doc/golang-repo

 创建文件夹 _str_，在 _str_ 文件夹下创建 _hello.go_ 文件，代码如下，这段代码引用了前面的 _quote_，在它的 _Hello_ 方法返回结果后面加上 _lin_ 返回给调用者。

    package str

    import (
    	"rsc.io/quote"
    )

    func LinHello() string {
    	return quote.Hello() + "lin"
    }

 提交代码并打上 _v0.1.0_ 的 _tag_，将 _tag_ 推送到 _github_。

    git commit -m "mymodule: changes for v0.1.0"
    git tag v0.1.0
    git push origin v0.1.0

 将模块同步到_Go Proxy_。提示成功后，即可在[pkg.go.dev](https://pkg.go.dev/)搜索到。

    go list -m github.com/linlan-doc/golang-repo

#### 4.3 使用包

 新建一个项目，引用刚发布的模块。在新项目下新建一个 _main_ 文件夹，在 _main_ 文件夹下新建 _test.go_ 文件，调用模块里面的 _LinHello_ 方法，观察控制台的输出结果。

:::caution

 需要新建项目，在打包项目直接引用会报错。

:::

```jsx title=test.go
package test

import (
	"fmt"

	repo "github.com/linlan-doc/golang-repo/str"
)

func main() {
	fmt.Println(repo.LinHello())
}
```

```jsx title=go.mod
module test

go 1.17

require github.com/linlan-doc/golang-repo v0.1.0

require (
	golang.org/x/text v0.0.0-20170915032832-14c0d48ead0c // indirect
	rsc.io/quote v1.5.2 // indirect
	rsc.io/sampler v1.3.0 // indirect
)
```

:::tip

 根据 _go_ 语言的标准，程序的入口是 _main_ 包下的 _main_ 方法

:::

### 5. 包与目录的关系

 上述例子里，模块、包、目录的关系非常混乱，本小节进行说明。

1.  发布的模块名为：_github.com/linlan-doc/golang-repo_，所以在 _go.mod_ 文件里面 _require_ 的模块名为 _github.com/linlan-doc/golang-repo_。
2.  发布的模块里 _str_ 包在目录 _str_ 下，所以 _import_ 的路径为 _github.com/linlan-doc/golang-repo/str_，即告诉 _go_ 编译器，_str_ 包在 _str_ 目录下，代码里给 _str_ 包重新命名为 _repo_。
3.  通过包名调用对应方法。

:::caution

1.  同一个目录下，不能申明两个包，否则编译器会报错。
2.  包名和目录名不需要一致，但建议保持一致，减少不必要的理解成本。
3.  同一个包下，直接用方法名进行调用。

:::

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
