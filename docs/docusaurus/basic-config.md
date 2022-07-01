---
sidebar_position:  2
title:             docusaurus基础配置
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

 _docusaurus_基于_nodejs_构建，要求_nodejs_的版本高于_16.14_，故使用_docusaurus_之前需要安装_nodejs_。

### 1. nodejs安装

<Tabs groupId="operating-systems">
  <TabItem value="win" label="Windows">前往<a href='https://nodejs.org/en/download/'>nodejs官网</a>下载安装</TabItem>
  <TabItem value="mac" label="macOS"><code>brew install node</code></TabItem>
  <TabItem value="Ubuntu" label="Ubuntu"><code>sudo apt-get install nodejs</code></TabItem>
</Tabs>

### 2. 初始化新的项目

 _docusaurus_支持一键新建项目，首先进入到你想要初始化项目的目录，执行

    npx create-docusaurus@latest my-website classic

 进入初始化的项目，执行以下命令即可启动服务。

    cd my-website
    npx run start

 _docusaurus_也支持生成静态站点，这样方便使用_nginx_作为服务器，而不需要单独启动_nodejs_的服务。使用下面命令即可编译，编译完成后在根目录会出现一个_build_的子目录。

    npx run build

### 3. 配置docusaurus.config.js

 _docusaurus.config.js_是整个站点的配置文件，_docusaurus_提供了非常丰富的配置，可以满足你的个性化诉求。

#### 3.1 配置gtag

 _gtag_可以用来监控页面的访问情况，_docusaurus_内置_gtag_，只需要修改以下配置即可。在_docusaurus.config.js_里面，有_presets_这个属性，将申请的_gtag_加入到_presets_即可。

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

 _docusaurus_没有内置_google adsense_，但添加_google adsense_非常容易，只需要将一段_js_代码加入到_head_标签即可。_docusaurus_支持添加自定义的_js_文件。在_docusaurus.config.js_里面添加属性_scripts_，将_google adsense_的地址添加进去。

    scripts: [{
        src: "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8766080864055711",
        crossorigin: "anonymous",
        async: true
      }
    ]

### 4. 使用第三方react

 借助_MDX_的能力，_docusaurus_支持在_md_文件里使用_react_组件。以[excel](https://www.npmjs.com/package/react-spreadsheet)，插件为例。先安装第三方组件，命令如下。

    npm install react react-dom scheduler react-spreadsheet

 在需要使用该三方组件的_md_文件头，引入该三方组件。

    import Spreadsheet from "react-spreadsheet";

 在需要使用的地方，直接使用这个组件。

    <Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserCurrentHost" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserAllHosts" }, { value: "当前用户，所有主机" }],[{ value: "$PROFILE.AllUsersCurrentHost" }, { value: "所有用户，当前主机" }],[{ value: "$PROFILE.AllUsersAllHosts" }, { value: "所有用户，所有主机" }]]} columnLabels={["变量","说明"]} />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
