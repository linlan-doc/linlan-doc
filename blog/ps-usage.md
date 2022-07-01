---
title:     Windows PowerShell如何使用
tags:      ['PowerShell']
keywords:  ['Windows PowerShell使用过程中问题汇总']
authors:   lin
---

import Spreadsheet from "react-spreadsheet";

 _PowerShell_是_MicroSoft_开发的一款跨平台的任务自动化解决方案，由命令行_Shell_、脚本语言和配置框架构成。_PowerShell_支持_KornShell_的核心语法，习惯_bash_的朋友非常容易切换到_PowerShell_，所以_PowerShell_是_Windows_上非常受欢迎的命令行工具。

### 1. 配置profile

 _PowerShell_支持个性化配置，同时定义了很多变量来方便用户对_profile_进行配置。

<Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserCurrentHost" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserAllHosts" }, { value: "当前用户，所有主机" }],[{ value: "$PROFILE.AllUsersCurrentHost" }, { value: "所有用户，当前主机" }],[{ value: "$PROFILE.AllUsersAllHosts" }, { value: "所有用户，所有主机" }]]} columnLabels={["变量","说明"]} />

 查看变量对应值

    $PROFILE | Get-Member -Type NoteProperty

 测试文件是否存在

    Test-Path -Path $PROFILE.AllUsersAllHosts

 新建文件，注意需要管理员权限

    New-Item -ItemType File -Path $PROFILE.AllUsersAllHost -Force

 修改文件

    notepad $PROFILE.AllUsersAllHosts

```jsx title="添加代理"
$ENV:HTTPS_PROXY='http://127.0.0.1:10809'
```
