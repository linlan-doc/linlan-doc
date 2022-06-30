---
sidebar_position:  2
title:             docusaurus基础配置
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

 `docusaurus`基于`nodejs`构建，要求`nodejs`的版本高于`16.14`，故使用`docusaurus`之前需要安装`nodejs`。

### 1. nodejs安装

<Tabs groupId="operating-systems">
  <TabItem value="win" label="Windows">前往<a href='https://nodejs.org/en/download/'>nodejs官网</a>下载安装</TabItem>
  <TabItem value="mac" label="macOS"><code>brew install node</code></TabItem>
  <TabItem value="Ubuntu" label="Ubuntu"><code>sudo apt-get install nodejs</code></TabItem>
</Tabs>

### 2. 初始化新的项目

 `docusaurus`支持一键新建项目，首先进入到你想要初始化项目的目录，执行

    npx create-docusaurus@latest my-website classic

 进入初始化的项目，执行以下命令即可启动服务。

    cd my-website
    npx run start

 `docusaurus`也支持生成静态站点，这样方便使用`nginx`作为服务器，而不需要单独启动`nodejs`的服务。使用下面命令即可编译，编译完成后在根目录会出现一个`build`的子目录。

    npx run build

### 3. 配置docusaurus.config.js

 `docusaurus.config.js`是整个站点的配置文件，`docusaurus`提供了非常丰富的配置，可以满足你的个性化诉求。

#### 3.1 配置gtag

 `gtag`可以用来监控页面的访问情况，`docusaurus`内置`gtag`，只需要修改以下配置即可。在`docusaurus.config.js`里面，有`presets`这个属性，将申请的`gtag`加入到`presets`即可。

    presets: [
        [
          'classic',
          /** @type {import('@docusaurus/preset-classic').Options} */
          ({
            docs: {
              sidebarPath: require.resolve('./sidebars.js'),
              // Please change this to your repo.
              // Remove this to remove the "edit this page" links.
            },
            blog: {
              showReadingTime: true,
              // Please change this to your repo.
              // Remove this to remove the "edit this page" links.
            },
            theme: {
              customCss: require.resolve('./src/css/custom.css'),
            },
            gtag: {
              trackingID: 'G-0QFZJ5DY3B',
              anonymizeIP: true,
            },
          }),
        ]
      ]

#### 3.2 配置google adsense

 `docusaurus`没有内置`google adsense`，但添加`google adsense`非常容易，只需要将一段`js`代码加入到`head`标签即可。`docusaurus`支持添加自定义的`js`文件。在`docusaurus.config.js`里面添加属性`scripts`，将`google adsense`的地址添加进去。

    scripts: [{
        src: "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8766080864055711",
        crossorigin: "anonymous",
        async: true
      }
    ]

### 4. 使用第三方react

 借助`MDX`的能力，`docusaurus`支持使用第三方的`react`组件。以[excel](https://www.npmjs.com/package/react-spreadsheet)，插件为例。先安装第三方组件，命令如下。

    npm install react react-dom scheduler react-spreadsheet

 在需要使用该三方组件的`md`文件头，引入该三方组件。

    import Spreadsheet from "react-spreadsheet";

 在需要使用的地方，直接使用这个组件。

    <Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserCurrentHost" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserAllHosts" }, { value: "当前用户，所有主机" }],[{ value: "$PROFILE.AllUsersCurrentHost" }, { value: "所有用户，当前主机" }],[{ value: "$PROFILE.AllUsersAllHosts" }, { value: "所有用户，所有主机" }]]} columnLabels={["变量","说明"]} />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
