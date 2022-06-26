---
title: golang开发环境搭建
sidebar_position:  2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

 编程语言的开发环境包含两部分：
1. `IDE`，用什么工具开发。
2. `SDK`，用什么编译和执行。

 `golang`主流的`IDE`是`Goland`（和`Intellij`是一家）。当然也可以选择`VS code`、`Atom`等编辑器，通过安装插件的方式实现轻量级的`IDE`。`Goland`一年的授权费为`200`美金，第二年有优惠，有条件的朋友可以购买证书。笔者使用的是`VS code`，因为`golang`团队为`VS code`开发了插件。

### 1. IDE下载

 前往[VS code官网](https://code.visualstudio.com/Download)，下载系统对应的安装程序。安装完成之后，前往[go插件地址](https://marketplace.visualstudio.com/items?itemName=golang.go)，点击下载会调起`VS code`进行插件安装。

### 2. SDK安装

 前往[golang官网](https://go.dev/doc/install)下载系统对应的安装程序，安装完成之后，在命令行执行`go version`确定`go`的安装路径已经添加到系统路径里面了。

### 3. 设置代理

 因为网络封锁的原因，有一些`golang`的包无法下载，这时候需要设置代理。搭建代理可以参考[建站全攻略](/docs/set-up-site/your-site-in-one)。

<Tabs groupId="operating-systems">
  <TabItem value="win-cmd" label="Windows的CMD">
    打开<code>CMD</code>（注意是<code>CMD</code>，不是<code>PowerShell</code>）,执行以下命令：<br />
    <code>
      set http_proxy=http://127.0.0.1:10809 <br />
      set https_proxy=http://127.0.0.1:10809 <br />
      go install -v xxx
    </code>
  </TabItem>
  <TabItem value="win-power" label="Windows的PowerShell">
    打开<code>PowerShell</code>,执行以下命令：<br />
    <code>
      $ENV:HTTPS_PROXY='http://127.0.0.1:10809' <br />
      $ENV:HTTP_PROXY='http://127.0.0.1:10809' <br />
      go install -v xxx
    </code>
  </TabItem>
  <TabItem value="other" label="其他类unix系统">
  <code>
    http_proxy=127.0.0.1:10809 go install -v xxx
  </code> <br />
    &emsp;如果失败，可能需要<code>https</code>的代理。<br />
  <code>
    https_proxy=127.0.0.1:10809 go install -v xxx
  </code>
  </TabItem>
</Tabs>

### 4. 使用VS code

 `VS code`无法新建文件夹，所以需要手动新建一个`golang`的文件夹，然后从`VS code`里打开这个文件夹。接着打开终端，在终端执行初始化`module`的命令:

    go mod init hello-world

![初始化module](./asserts/golang-1.png)

 在`golang`文件夹下新建一个文件夹`main`，在`main`文件夹下新建`hello-world.go`，这个时候`VS code`会提示你有些插件没有安装，如果直接安装会失败，请使用代理试试。

![新建文件](./asserts/golang-2.png)

### 5. 编写hello world

 和其他编程语言一样,`golang`的入口函数也是`main`，接下来我们编写一个打印`hello world`的程序，并且运行。

    package main
    import "fmt"

    func main() {
    	fmt.Print("hello world")
    }

![运行程序](./asserts/golang-3.png)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
