---
title:     Windows PowerShell使用指南
tags:      ['PowerShell']
keywords:  ['Windows PowerShell使用过程中问题汇总']
authors:   lin
---

import Spreadsheet from "react-spreadsheet";

 _PowerShell_ 是 _MicroSoft_ 开发的一款跨平台的任务自动化解决方案，由命令行 _Shell_、脚本语言和配置框架构成。_PowerShell_ 支持 _KornShell_ 的核心语法，习惯 _bash_ 的朋友非常容易切换到 _PowerShell_，所以 _PowerShell_ 是 _Windows_ 上非常受欢迎的命令行工具。

### 1. 配置profile

 _PowerShell_ 支持个性化配置，同时定义了很多变量来方便用户对 _profile_ 进行配置。

<Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserCurrentHost" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserAllHosts" }, { value: "当前用户，所有主机" }],[{ value: "$PROFILE.AllUsersCurrentHost" }, { value: "所有用户，当前主机" }],[{ value: "$PROFILE.AllUsersAllHosts" }, { value: "所有用户，所有主机" }]]} columnLabels={["变量","说明"]} hideRowIndicators={true} />

 查看变量对应值

    $PROFILE | Get-Member -Type NoteProperty

 测试文件是否存在

    Test-Path -Path $PROFILE.AllUsersAllHosts

 新建文件，注意需要管理员权限

    New-Item -ItemType File -Path $PROFILE.AllUsersAllHost -Force

 修改文件

    notepad $PROFILE.AllUsersAllHosts

 在 _profile_ 里新增代理配置如下，这样启动 _PowerShell_ 时默认会将 _<http://127.0.0.1/10809>_ 作为 _Https_ 的代理。

```jsx title="添加代理"
$ENV:HTTPS_PROXY='http://127.0.0.1:10809'
```

### 2. scp免密拷贝

 发布站点到服务器时，需要将本地打包好的内容拷贝到远程服务器上。_PowerShell_ 支持 _scp_ 命令，可以参考[github使用](/blog/github-usage)一文，将本地的 _pub key_ 加到远程服务器的信任 _key_ 中，这样拷贝时不用输入密码。

    cat ~/.ssh/id_rsa.pub | ssh username@server.address.com 'cat >> ~/.ssh/authorized_keys'

 在 _profile_ 里面定义下面两个变量:

    $SSH_PUB_KEY='添加到远程服务器信任的key'
    $REMOTE_SERVER='远程服务器的地址'

 [docusaraus](/docs/docusaurus/basic-config)编译生成的站点可以通过下面命令完成部署。

    scp -i $SSH_PUB_KEY -r build/ $REMOTE_SERVER

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
