---
title:     react-spreadsheet使用指南
tags:      ['react','spreadsheet','excel组件']
keywords:  ['react-spreadsheet组件使用']
authors:   lin
---

 [react-spreadsheet](https://www.npmjs.com/package/react-spreadsheet)是 _react_ 的 _excel_ 组件，[官方文档](https://iddan.github.io/react-spreadsheet/docs/)内容比较少，本文结合[Spreadsheet](https://github.com/iddan/react-spreadsheet/blob/42ce608/src/Spreadsheet.tsx#L126)的源代码，介绍一些该表格组件的几个非常重要的属性。

### 1. 列标签

 列标签是一个字符数组，默认值为：[A,B,C,...]。属性名称为：`columnLabels`。举个例子：

    <Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }]]} columnLabels={["变量","说明"]}  />

### 2. 行标签

 和列标签一样，行标签也是一个字符数组，默认值为：[1,2,3,...]，属性名称为：`rowLabels`，例子同上。

### 3. 隐藏行标签

 隐藏行标签是一个布尔类型，默认值为`false`，属性名称为：`hideRowIndicators`

### 4. 隐藏列标签

 隐藏列标签是一个布尔类型，默认值为`false`，属性名称为：`hideColumnIndicators`

### 5. value为==时渲染失败

 在`==`前加上空格即可
