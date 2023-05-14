---
title:     如何在个人计算机上运行open ai的whisper模型
tags:      ['openai','chatgpt']
keywords:  ['个人计算机','大模型','人工智能']
authors:   lin
---

import Image from '@theme/IdealImage';

 随着 _chatgpt_ 的发布，_open ai_ 火遍全网，但它所需要的算力也将普通人拒之门外。_Github_ 上一位保加利亚的大神实现了 _open ai_ 的模型，声称可以在个人计算机上运行，本文基于[ggerganov
/whisper.cpp](https://github.com/ggerganov/whisper.cpp)这个仓库，在 _Windows_ 上运行 _whisper_ 模型。

#### 1. cygwin & MinGW 安装

 该模型编译依赖 _gcc_ ，故需要先在 _Windows_ 上安装 _gcc_ 的编译环境。 _cygwin_ 使用广泛，故本文采用 _cygwin_。

 前往[cygwin](https://cygwin.com/install.html)官网下载安装包，安装到下面界面的时候，记得搜索以下 _gcc_，找到 _mingw_ 对应的包，只需要选中 _gcc-g++_ 即可，它会自动安装对应的依赖。

<Image img={require('./asserts/cygwin.png')} alt="搜索gcc相关包" />

 安装完成之后，打开 _cygwin_，检查 _/usr/bin/_ 目录下是否已经安装好 _gcc_ 了。

#### 2. 模型下载

 将 _ggml_ 格式的模型下载到本地。打开 _Windows Power Shell_，进入到下载文件的 _models_ 目录下，执行以下命令:

    .\download-ggml-model.cmd base.en

#### 3.执行编译

 因为 _cygwin_ 下 _gcc_ 和 _g++_ 名字问题，在执行 _make_ 命令时需要指定参数。

    make CC=x86_64-w64-mingw32-gcc CXX=x86_64-w64-mingw32-g++

:::info

 如果提示找不到 _dll_，思路如下：

1.  确认 _dll_ 是否已经安装，通过 _cygwin_ 的安装程序即可查看。


2.  将下面变量放到 _.bashrc_ 文件里


    export LD_LIBRARY_PATH=/usr/lib
    export PATH=$PATH:/usr/x86_64-w64-mingw32/sys-root/mingw/bin

:::

#### 4. 执行 whisper

    ./main -f samples/jfk.wav
