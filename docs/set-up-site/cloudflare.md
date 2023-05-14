---
sidebar_position: 2
title: 利用cloudflare免费cdn为你的站点加速
keywords: ['国外免费cdn','防止ddos攻击','cloudflare接入']

toc_max_heading_level: 4
---

import Image from '@theme/IdealImage';

  _cdn_(_content delivery network_)可以将站点的静态资源缓存到全球多个不同机房，这样不同国家的用户访问站点时可以从离用户最近的机房获取到数据，从而极大的提升用户体验。

 [cloundflare](https://www.cloudflare.com/)是一家著名的 _cdn_ 提供厂商，它提供了免费的 _cdn_ 托管服务，该服务套餐中除了 _cdn_，还包含了3-7层的 _DDos_ 攻击防护服务，本文介绍如何将自己的站点接入 _cloudflare_。

### 1. 注册账号

 前往[官网](https://www.cloudflare.com/)按照指引注册账号即可。

### 2. 添加你的站点

 点击添加站点，输入你的站点之后，弹出选择服务的框，选择 _Free_ 即可。可以看到 _Free_ 的 _Core Features_ 是防止 _DDos_ 攻击和全球范围的 _cdn_ 。

<Image img={require('./asserts/set-up-site13.png')} alt="cloudflare的服务" />

### 3. 确认DNS记录

 选择 _Free Plan_ 之后，_cloudflare_ 会对你的域名进行扫描，显示域名解析的记录，确认即可，你可以根据自己需要选择只代理部分域名。例如：下图中没有对 _api_ 进行代理。

<Image img={require('./asserts/set-up-site14.png')} alt="DNS确认" />

### 4. 修改nameserver

 最后一步是将域名原来的 _nameserver_ 换成 _cloudflare_ 的。这需要前往域名供应商的后台进行修改，以[namesilo](https://www.namesilo.com/?rid=2ddf330)为例，选中域名，点击 _Change NameServers_ 即可。

<Image img={require('./asserts/set-up-site15.png')} alt="cloudflare提供的nameserver" />

<Image img={require('./asserts/set-up-site16.png')} alt="修改nameserver" />

### 5. warp+使用

 前往[warp](https://1.1.1.1/)官网下载对应客户端版本，目前测试下来 _windows_ 版可以正常连接，但安卓无法使用。

 电报上有免费的 _warp+_ 流量可以领取。先关注 _Warp Plus_ ,然后前往 _Warp+ Bot_ 点击 _start_， 回答它的一个非常简单的问题，即可生成一个 _key_，这个 _key_ 的流量是几十 _PB_，一般用不完。获得 _key_ 之后在 _warp_ 客户端进行替换即可，注意替换的时候可能是需要 _VPN_ 的。

 有了 _warp_，你可以 _ssh_ 到被墙的 _vps_ 上，解决了一大痛点。

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
