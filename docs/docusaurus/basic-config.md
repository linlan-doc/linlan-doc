---
sidebar_position:  2
title:             docusaurus基础配置
toc_max_heading_level: 4

keywords: ['docusaurus配置','个人建站','react','网站模板','docusaurus使用','docusaurus adsense']
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';


 _docusaurus_ 基于 _nodejs_ 构建，要求 _nodejs_ 的版本高于16.14，故使用 _docusaurus_ 之前需要安装 _nodejs_。

### 1. nodejs安装

<Tabs groupId="operating-systems">
  <TabItem value="win" label="Windows">

 前往[nodejs官网](https://nodejs.org/en/download)下载安装。

  </TabItem>
  <TabItem value="mac" label="macOS">

    brew install node

  </TabItem>

  <TabItem value="Ubuntu" label="Ubuntu">

      sudo apt-get install nodejs

  </TabItem>
</Tabs>

### 2. 初始化新的项目

 _docusaurus_ 支持一键新建项目，首先进入到你想要初始化项目的目录，执行

    npx create-docusaurus@latest my-website classic

 进入初始化的项目，执行以下命令即可启动服务。

    cd my-website
    npx run start

 _docusaurus_ 也支持生成静态站点，这样方便使用 _nginx_ 作为服务器。使用下面命令即可编译，编译完成后在根目录会出现一个 _build_ 的子目录。

    npx run build

### 3. 配置docusaurus.config.js

 _docusaurus.config.js_ 是整个站点的配置文件，它提供了丰富的配置来满足用户的个性化诉求。

#### 3.1 配置gtag

 _gtag_ 可以用来监控页面的访问情况，_docusaurus_ 内置 _gtag_ ，在 _presets_ 属性里，加入 _gtag_ 配置即可。

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

 _docusaurus_ 没有内置 _google adsense_，但添加 _google adsense_ 非常容易，只需要将一段 _js_ 代码加入到 _head_ 标签即可。_docusaurus_ 支持添加自定义的 _js_ 文件。在 _docusaurus.config.js_ 里面添加属性 _scripts_，将 _google adsense_ 的地址添加进去。

    scripts: [{
        src: "https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8766080864055711",
        crossorigin: "anonymous",
        async: true
      }
    ]

#### 3.3 使用公式

 _docusaurus_ 使用[KaTex](https://katex.org/)渲染公式，引入 _KaTex_ 步骤如下。

1.  安装插件

<Tabs groupId="katex">
  <TabItem value="npm" label="npm">

    npm install --save remark-math@3 rehype-katex@5 hast-util-is-element@1.1.0

  </TabItem>
  <TabItem value="yarn" label="yarn">

    yarn add remark-math@3 rehype-katex@5 hast-util-is-element@1.1.0

  </TabItem>

</Tabs>

2.  引入插件

 将插件引入`docusaurus.config.js`

    const math = require('remark-math');
    const katex = require('rehype-katex');

3.  使用插件

 将插件加入到`presets`的`doc`属性里

    remarkPlugins: [math],
    rehypePlugins: [katex],

 因为第2步中定义了`math`和`katex`两个变量，所以上面配置等同于：

    remarkPlugins: [require('remark-math')],
    rehypePlugins: [require('rehype-katex')],

 如果提示以下错误，需要加上`{strict:false}`。

> ValidationError: "remarkPlugins[1]" does not look like a valid MDX plugin config. A plugin config entry should be one of:
>
> -   A tuple, like `[require("rehype-katex"), { strict: false }]`, or
> -   A simple module, like `require("remark-math")`

    remarkPlugins: [math,{strict:false}],
    rehypePlugins: [katex],

4.  引入css

 在`stylesheets`配置下，添加 _KaTex_ 的 _css_ 文件。

    stylesheets: [
      {
        href: 'https://cdn.jsdelivr.net/npm/katex@0.13.24/dist/katex.min.css',
        type: 'text/css',
        integrity:
          'sha384-odtC+0UGzzFL/6PNoE8rX/SPcQDXBJ+uRepguP4QkPCm2LBxH3FA3y+fKSiJ+AmM',
        crossorigin: 'anonymous',
      },
    ],

 修改之后，`docusaurus.config.js`变成

    const math = require('remark-math');
    const katex = require('rehype-katex');

    module.exports = {
      title: 'Docusaurus',
      tagline: 'Build optimized websites quickly, focus on your content',
      presets: [
        [
          '@docusaurus/preset-classic',
          {
            docs: {
              path: 'docs',
              remarkPlugins: [math],
              rehypePlugins: [katex],
            },
          },
        ],
      ],
      stylesheets: [
        {
          href: 'https://cdn.jsdelivr.net/npm/katex@0.13.24/dist/katex.min.css',
          type: 'text/css',
          integrity:
            'sha384-odtC+0UGzzFL/6PNoE8rX/SPcQDXBJ+uRepguP4QkPCm2LBxH3FA3y+fKSiJ+AmM',
          crossorigin: 'anonymous',
        },
      ],
    };


:::caution

 `docusaurus.config.js`的 _css_ 配置是全局的，意味着所有页面加载时都会加载这个 _css_ 文件，这样做并不合理。可以只在使用公式的 _md_ 文件开头，加上：

    <head>
      <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.13.24/dist/katex.min.css" integrity="sha384-odtC+0UGzzFL/6PNoE8rX/SPcQDXBJ+uRepguP4QkPCm2LBxH3FA3y+fKSiJ+AmM" crossorigin="anonymous" />
    </head>

:::


5.  语法

 行内公式用`$`标识，公式块用`$$`。举个例子

    $i^2 =-1      //行内

    $$           //公式块
    I = \int_0^{2\pi} \sin(x)\,dx
    $$


:::caution

公式不能放在代码块里

:::

#### 3.4 响应式图片

 _plugin-ideal-image_ 插件支持生成响应式、懒加载的图片，极大的增强用户体验。插件安装步骤如下：

1.  安装插件

<Tabs groupId="IdealImage">
  <TabItem value="npm" label="npm">

    npm install --save @docusaurus/plugin-ideal-image

  </TabItem>
  <TabItem value="yarn" label="yarn">

    yarn add @docusaurus/plugin-ideal-image

  </TabItem>
</Tabs>

2.  修改docusaurus.config.js


    module.exports = {
      plugins: [
        [
          '@docusaurus/plugin-ideal-image',
          {
            quality: 70,
            max: 1030, // max resized image's size.
            min: 640, // min resized image's size. if original is lower, use that size.
            steps: 2, // the max number of images generated between min and max (inclusive)
            disableInDev: false,
          },
        ],
      ],
    };

3.  使用插件

 在 _md_ 文件里使用即可。

    import Image from '@theme/IdealImage';
    import thumbnail from './path/to/img.png';

    // your React code
    <Image img={thumbnail} />

    // or
    <Image img={require('./path/to/img.png')} />

:::tip

插件仅支持PNG和JPG格式的图片

:::

### 4. 使用第三方react

 借助 _MDX_ 的能力，_docusaurus_ 支持在 _md_ 文件里使用 _react_ 组件。以[excel](https://www.npmjs.com/package/react-spreadsheet)插件为例。先安装第三方组件，命令如下。

    npm install react react-dom scheduler react-spreadsheet

 在需要使用该三方组件的_md_文件头，引入该三方组件。

    import Spreadsheet from "react-spreadsheet";

 在需要使用的地方，直接使用这个组件。

    <Spreadsheet data={[[{ value: "$PROFILE" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserCurrentHost" }, { value: "当前用户，当前主机" }],[{ value: "$PROFILE.CurrentUserAllHosts" }, { value: "当前用户，所有主机" }],[{ value: "$PROFILE.AllUsersCurrentHost" }, { value: "所有用户，当前主机" }],[{ value: "$PROFILE.AllUsersAllHosts" }, { value: "所有用户，所有主机" }]]} columnLabels={["变量","说明"]} />

### 5. 配置docs

#### 5.1 配置文章的目录结构

 _docusaurus_ 默认会将文章里的 _Head_ 抽取出来，作为文章的目录结构，并且只抽取2和3级标题。同时它也支持用户在文章开头的配置里指定是否显示目录结构，以及抽取的标题的级别。

    ---
    hide_table_of_contents: true
    toc_min_heading_level: 1
    toc_max_heading_level: 4
    ---

### 6. 修改category路径

 _doc_ 目录下，每一个文件夹是一个 _category_ ，文件夹下的 _category.json_ 是该 _category_ 的配置。默认情况下，_category_ 的路径为 _label_ 标签值，如果标签包含中文，那么 _category_ 路径也会包含中文，不是非常友好，在 _category.json_ 下加上 _slug_ 即可。

    {
      "label": "docusaurus使用",
      "position": 3,
      "link": {
        "type": "generated-index",
        slug: "/category/docusaurus"
      }
    }

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
