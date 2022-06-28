---
title:                                                 Atom编辑器使用
tags:                                                  ['Atom','IDE','nodejs']
keywords:                                              ['Atom编辑器']
authors:                                               lin
---

 `Atom`是一款免费、开源充满黑科技的文本编辑器，最近开发一些`nodejs`的项目时，尝试了一下这款编辑器，记录下使用过程，方便以后查阅，也希望能帮助到有需要的朋友。

### 1. 下载与安装

 `Atom`的[官方网站](https://atom.io/)和[github](https://github.com/atom/atom)首页有详细的安装教程，这里就不再赘述。

### 2. 设置代理

 因为网络封锁的原因，有时需要代理才能下载某些插件，搭建代理的方式可以参考[建站全攻略](/docs/set-up-site/your-site-in-one)。先关闭`Atom`，打开终端，执行下面命令。

    apm config set http-proxy http://127.0.0.1:10886

### 3. 插件安装

 `Atom`是一款轻量级的编辑器，只安装了使用最频繁的插件，如果想作为`ide`，需要手动安装一些插件，在`File-Settings-Install`里面可以搜索需要安装的插件，下面列举一些非常常用的插件。

-   `atom-beautify`代码格式化

-   `platformio-ide-terminal`打开终端的插件
-   `vim-mode`，如果搜不到，可以使用`apm`命令进行安装`apm install vim-mode`
-   `file-templates`文件模板，在写文档时，`md`文件中很多内容都是重复的，可以通过模板来创建新的文件。不过这个插件并不是通过文件的后缀名来匹配模板，而是创建的时候手动的指定从哪个模板创建。