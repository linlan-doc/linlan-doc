---
title: go开发环境搭建
sidebar_position:  2

keywords: ['go编程','go开发环境搭建','go语言学习','go教程']
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import Image from '@theme/IdealImage';

 编程语言的开发环境包含两部分：

1.  _IDE_，用什么工具开发。
2.  _SDK_，用什么编译和执行。

 _go_ 主流的 _IDE_ 是 _Goland_（和 _Intellij_ 是一家）。当然也可以选择 _VS code_、_Atom_ 等编辑器，通过安装插件的方式实现轻量级的 _IDE_。_Goland_ 一年的授权费为200美金，第二年有优惠，有条件的朋友可以购买授权。笔者使用的是 _VS code_，因为 _go_ 团队为 _VS code_ 开发了插件。

### 1. IDE下载

 前往[VS code官网](https://code.visualstudio.com/Download)，下载系统对应的安装程序。安装完成之后，前往[go插件地址](https://marketplace.visualstudio.com/items?itemName=golang.go)，点击下载会调起 _VS code_ 进行插件安装。

### 2. SDK安装

 前往[go官网](https://go.dev/doc/install)下载系统对应的安装程序，安装完成之后，在命令行执行 _go version_ 确定 _go_ 的安装路径已经添加到系统路径里面了。

### 3. 设置代理

 因为网络封锁的原因，有一些 _go_ 的包无法下载，这时候需要设置代理。搭建代理可以参考[建站全攻略](/docs/set-up-site/your-site-in-one)。

<Tabs groupId="operating-systems">
  <TabItem value="win-cmd" label="Windows的CMD">

 打开 _CMD_（注意是 _CMD_，不是 _PowerShell_）,执行以下命令：

    set http_proxy=http://127.0.0.1:10809
    set https_proxy=http://127.0.0.1:10809
    go install -v xxx

  </TabItem>
  <TabItem value="win-power" label="Windows的PowerShell">

 打开 _PowerShell_，执行以下命令：

    $ENV:HTTPS_PROXY='http://127.0.0.1:10809'
    $ENV:HTTP_PROXY='http://127.0.0.1:10809'
    go install -v xxx

  </TabItem>
  <TabItem value="other" label="其他类unix系统">

    http_proxy=127.0.0.1:10809 go install -v xxx

 如果失败，可能需要_https_的代理。

    https_proxy=127.0.0.1:10809 go install -v xxx

  </TabItem>
</Tabs>

:::tip

 _Power Shell_ 可以通过修改 _profile_ 来设置代理，参考[PowerShell使用教程](../../blog/ps-usage)

:::

### 4. 使用VS code

 _VS code_ 无法新建文件夹，所以需要手动新建一个 _go_ 的文件夹，然后从 _VS code_ 里打开这个文件夹。接着打开终端，在终端执行初始化 _module_ 的命令:

    go mod init hello-world


<Image img={require('./asserts/golang-1.png')} alt="初始化module" />


 在 _go_ 文件夹下新建一个文件夹 _main_，在 _main_ 文件夹下新建 _hello-world.go_，这个时候 _VS code_ 会提示你有些插件没有安装，如果直接安装会失败，请使用代理试试。


<Image img={require('./asserts/golang-2.png')} alt="新建文件" />


### 5. 编写hello world

 _go_ 的程序入口为 _main_ 包下面的 _main_ 方法，接下来我们编写一个打印 _hello world_ 的程序，并且运行。

    package main
    import "fmt"

    func main() {
    	fmt.Print("hello world")
    }


<Image img={require('./asserts/golang-3.png')} alt="运行程序" />

 文件的目录结构如下：

    root
    ├── main
    │   └── hello-world.go
    ├── go.mod
    └── go.sum

### 6. 汇编代码

 _go_ 编译工具支持打印汇编代码，在学习的过程中可以结合汇编代码加深对 _go_ 的理解，下面命令将 _main.go_ 的汇编代码写入 _main.s_ 文件中。

    go tool compile -S -N main.go >main.s



[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
