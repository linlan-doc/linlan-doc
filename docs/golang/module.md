---
title: go模块
sidebar_position:  3
---

 模块是_go_代码包的集合，模块根目录下的_go.mod_文件定义了模块的路径、依赖等信息。

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

 在_golang_开发过程中，需要用到其他人开发的模块。像_Maven_、_NPM_这些包管理工具，都有中心仓库，开发者开发好软件包之后，提交到中心仓库，其他人就可以使用了。

 _golang_的包管理思路不同于以上，_golang_采用半中心化的思路。任意的_git_仓库都可以作为_golang_的包仓库，只要仔细观察你就会发现不同的包会来自不同的地址（这点和_Maven_、_NPM_非常不同）。

> By default, the go command may download modules from <https://proxy.golang.org>. It may authenticate modules using the checksum database at <https://sum.golang.org>. Both services are operated by the Go team at Google.The privacy policies for these services are available at <https://proxy.golang.org/privacy> and <https://sum.golang.org/privacy>, respectively.
>
> The go command's download behavior may be configured using GOPROXY, GOSUMDB,GOPRIVATE, and other environment variables

 当执行_go get_命令的时候，_golang_首先检查本地是否有需要安装的包，如果没有找到，就会去服务器下载。下载地址取决于_GOPROXY_这个变量值，_GOPROXY_默认值为：_<https://proxy.golang.org>, direct_。这个默认值的含义是，如果在_<https://proxy.golang.org>_没有找到需要安装的包，就去包名对应_URL_下载。

 _proxy.golang.org_是由_google_运营的模块镜像仓库，它会从源服务器同步模块到_proxy.golang.org_，当用户安装时，优先从_proxy.golang.org_下载。

:::caution

中国大陆屏蔽了_proxy.golang.org_，只能使用其他的镜像仓库，例如：[goproxy.cn](https://github.com/goproxy/goproxy.cn/blob/master/README.zh-CN.md)。使用的时候，修改_GOPROXY_的值即可。

    $ go env -w GO111MODULE=on
    $ go env -w GOPROXY=https://goproxy.cn,direct

:::

 接下来，看看执行_go get rsc.io/quote/v4_后，发生了什么。

1.  _go.mod_文件里面多了_require_的内容。
2.  多了文件_go.sum_

 _require_比较好理解，在执行_go get_的时候，将包的依赖加到_go.mod_中。_go.sum_文件的每一行数据包括包名、包版本和对应的_hash_值，它用来保证之后下载的包与第一次下载的包一致，防止包被恶意篡改。

 在代码里面引入_quote_，成功调用_quota.Hello()_方法。

![引入quote](./asserts/golang-4.png)

:::tip

 如果无法下载quote包，清参考[golang环境搭建](/docs/golang/set-up)里面的代理设置。

:::

### 3. 升级包依赖

 _golang_的包版本号的格式类似于_v0.1.2_，其中_0_是大版本号，_1_是小版本号，_2_为补丁。执行_go list -m all_时，发现_golang.org/x/text_使用的是没有_tag_的版本，需要升级到最新的_tag_版本。你会发现_go.mod_里，_require_部分相应的发生了变化，同时_go.sum_里新增了最新版的记录。

    go get golang.org/x/text

![升级包](./asserts/golang-5.png)

 细心的小伙伴可能发现，包后面有_indirect_的标注，这个_indirect_表示不直接依赖这个包。例如_A -> B, B -> C_，那么_A_就间接依赖_C_。但是上述代码里面直接引用了_quote_包，为什么它也是_indirect_？这可能是先_go get_，后在代码里面引用的缘故，执行_go mod tidy_即可。

![执行go mod tidy](./asserts/golang-6.png)

### 4. 发布包

 _go_支持用户发布自己开发的模块。前面提到，_Go Proxy_并不是一个中心化的仓库，它会同步用户发布的模块，所以需要用户先将模块发布到互联网上。最方便的发布方式是使用_github_。不清楚如何使用_github_的可以参考[github使用指南](../../blog/github-usage)。

#### 4.1 新建代码仓库

 在_github_上创建一个空的仓库，将空仓库拉到本地。

#### 4.2 编写包代码

 新建模块，其中_linlan-doc_是你的账号名，_golang-repo_是你的仓库名

    go mod init github.com/linlan-doc/golang-repo

 创建文件夹_str_，在_str_文件夹下创建_hello.go_文件，代码如下，这段代码引用了前面的_quote_，在它的_Hello_方法返回结果后面加上_lin_返回给调用者。

    package str

    import (
    	"rsc.io/quote"
    )

    func LinHello() string {
    	return quote.Hello() + "lin"
    }

 提交代码并打上_v0.1.0_的_tag_，将_tag_推送到_github_。

    git commit -m "mymodule: changes for v0.1.0"
    git tag v0.1.0
    git push origin v0.1.0

 将模块同步到_Go Proxy_。提示成功后，即可在[pkg.go.dev](https://pkg.go.dev/)搜索到。

    go list -m github.com/linlan-doc/golang-repo

#### 4.3 使用包

 新建一个项目，引用刚发布的模块。在新项目下新建一个_main_文件夹，在_main_文件夹下新建_test.go_文件，调用模块里面的_LinHello_方法，观察控制台的输出结果。

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

 根据_go_语言的标准，程序的入口是_main_包下的_main_方法

:::

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
