---
sidebar_position: 1
title: 搭建站点全攻略（多图）
keywords: ['站点搭建','域名购买','vpn搭建']
toc_max_heading_level: 4
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import Image from '@theme/IdealImage';

:::note

因中国大陆审查严格，故本攻略仅针对海外建站。

:::

### 准备工作

 购买海外的云服务产品可能需要一张支持美元的信用卡，所以需要提前申请。

### 1. 服务器选择

 海外的服务器提供商主要有：_Google Cloud_、_Amazon AWS_、_Vultr_、_Digital Ocean_ 等。_Google Cloud_ 和 _Amazon AWS_ 都提供免费试用的服务，但这两家产品都偏贵，而且网络传输并不免费，在试用的时候需要格外小心。

 个人建站推荐使用 _Vultr_ 和 _Digital Ocean_，这两家最便宜的服务器只需要5 _$_/月。笔者使用比较多的是[Vultr](https://www.vultr.com/?ref=8912579)，新用户注册可以领取100 _$_ 的体验券，但这张体验券有效期只有1个月，即最多白嫖1个月。

 在选择将服务器部署到哪个国家之前，需要对该国的网络进行测速。_ping test_ 的工具很多，例如[站长之家](https://ping.chinaz.com/)的 _ping_ 工具。云服务器厂商提供了测试的 _ip_，在 _Google_ 上搜索 _xxx ping test_ 就能搜到云厂商的测速页面，例如_Vultr_的[ping test](https://sgp-ping.vultr.com/)。你可以选择一个国家，获取到测试 _ip_，然后用 _ping_ 工具进行测试。从笔者使用情况看[Vultr](https://www.vultr.com/?ref=891257)韩国的服务器比较好，国内的延迟大约150 _ms_。

 接下来需要选择操作系统镜像，选择 _Clound Compute_，这里面的套餐最低5 _$_/月，同时包含了1000 _G_ 的流量，个人站点足够了。笔者使用的是 _Ubuntu_ 的镜像，因此本文基于 _Ubuntu_ 进行编写的。

<Image img={require('./asserts/set-up-site2.png')} alt="镜像选择" />

### 2. 登录服务器

 云服务器厂商提供了登录云服务器的工具，以[Vultr](https://www.vultr.com/?ref=891257)为例，进入服务器实例详情，点击右上角 _View Console_ 即可登录到服务器。当然你也可以使用 _ssh_ 命令进行登录，在实例详情页面可以找到 _ip_ 地址、端口以及密码。

<Image img={require('./asserts/set-up-site1.png')} alt="登录到服务器" />

 如果是 _windows_ 用户，可以安装 _WinScp_ 这款工具，拷贝文件非常方便，同时也可以集成 _putty_。

### 3. 安装nginx

 _nginx_ 是一款风靡全球的反向代理服务器，个人站点多数是静态站点，用 _nginx_ 作为服务器非常方便。

    sudo apt-get install nginx

 安装完成之后，需要配置域名。以 _linlan.xyz_ 为例，在 _/etc/nginx/conf.d_ 目录下，新建 _linlan.conf_，这个配置文件的内容非常简单：指定了端口号和域名。

    server {
            server_name linlan.xyz;
            listen 80;
    }

 接下来需要重启一下 _nginx_，让 _linlan.conf_ 配置生效。

    nginx -t //测试一下配置是否正常
    nginx -s reload //重启服务

:::tip

 访问`http://example.com/foo`，nginx会301到`http://example.com/foo/`。为了避免301，需要加上以下配置：

    try_files $uri $uri/index.html $uri/ =404;

:::

### 4. 购买域名

 域名是我们站点的名字，海外有一些知名的域名供应商，例如[Godaddy](https://www.godaddy.com)、[Domain](https://www.domain.com/)、[NameSilo](https://www.namesilo.com/?rid=2ddf330gz)，推荐使用[NameSilo](https://www.namesilo.com/?rid=2ddf330g)，
[NameSilo](https://www.namesilo.com/?rid=2ddf330g)首年有优惠，续费也没那么多套路，几十块一年，_DNS_ 解析不需要额外收费，并且支持支付宝付款。

#### 4.1 配置DNS

 购买域名后，需要将域名指向云服务器。在[NameSilo](https://www.namesilo.com/?rid=2ddf330g)的首页，点击右上角的 _Manage Domain_（如下图)，进入到域名列表，找到你想要绑定的域名，点击 _Manage DNS for this domain_，进入 _DNS_ 配置页面。

<Image img={require('./asserts/set-up-site3.png')} alt="管理域名" />

 以本站为例，_linlan.xyz_ 是一个二级域名，如果要让 _linlan.xyz_ 指向购买的服务器，需要新建一个类型 _A_ 的域名解析记录。_HostName_ 选`@.linlan.xyz`，_IP Address_ 填写云服务器对外的 _IP_ 地址。_NameSilo_ 生效的时间比较长，需要等一会域名解析才能生效，使用 _ping_ 命令可查看域名解析是否已经生效。

<Image img={require('./asserts/set-up-site4.png')} alt="配置解析记录" />

### 5. 检查服务器http和https端口

 域名解析生效之后，在浏览器输入购买的域名，就能够看到 _nginx_ 的欢迎页面了。如果 _ping_ 和 _ssh_ 都正常，但是页面无法正常打开，这时需要检查服务器是否允许访问80 _(http)_ 和443 _(https)_ 端口。如果端口没有打开，可以通过下面命令开启，开启之后再测试一下页面是否能够正常访问。

    ufw status verbose //查看防火墙状态，确认端口是不是关闭
    sudo ufw allow 80 //打开80端口
    sudo ufw allow 443 //打开443端口

### 6. HTTPS证书申请

 _https_ 已经是站点的标配了，_Let’s Encrypt_ 提供免费的 _https_ 证书，可以为你的站点申请一个。

#### 6.1 安装_Encrypt Client_

<Tabs groupId="operating-systems">
  <TabItem value="python2" label="python2"><code>sudo apt-get install certbot <br/>apt-get install python-certbot-nginx</code></TabItem>
  <TabItem value="python3" label="python3"><code>sudo apt-get install certbot<br/>apt-get install python3-certbot-nginx</code></TabItem>
</Tabs>

#### 6.2 nginx配置（见章节3）

#### 6.3 生成证书

    sudo certbot --nginx -d example.com

 上述命令执行完成之后，你可以前往 _/etc/nginx/conf.d_ 文件夹下，查看站点的配置。你会发现配置的内容被 _cerbot_ 修改过，增加了 _https_ 的内容。打开浏览器，使用 _https_ 请求站点，确定 _https_ 证书是否正确安装。

#### 6.4 刷新证书

 _Let's Encrypt_ 生成的证书有效期为90天，需要定时的刷新。

-   打开_crontab_

        crontab -e

-   新建自动刷新命令

        0 12 * * * /usr/bin/certbot renew --quiet

### 7. trojan安装

 _VPN_ 的协议有很多，_SS_、_Project V_ 和 _Trojan_，笔者比较推荐 _Trojan_，相对稳定，防干扰的能力出色。

    sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"

 _trojan_ 会监听443端口，如果不是 _trojan_ 协议的流量，_trojan_ 会将请求转发到80端口，故 _nginx_ 需关掉 _https_ 的端口，只需将章节6中 _cerbot_ 增加的 _https_ 内容去掉即可，但需要记录证书和 _key_ 的路径，这两个配置项在配置 _trojan_ 的时候需要用到。

    server {
            server_name linlan.xyz;
            listen 80;
    }

 修改完 _nginx_ 的配置文件之后，重启一下 _nginx_。打开 _trojan_ 的配置文件，_Ubuntu_ 下路径为 _/usr/local/etc/trojan/config.json_，修改三个地方：密码，_cert_和_key_。

    {
        "run_type": "server",
        "local_addr": "0.0.0.0",
        "local_port": 443,
        "remote_addr": "127.0.0.1",
        "remote_port": 80,
        "password": [
            "设置一个访问trojan的密码"
        ],
        "log_level": 1,
        "ssl": {
            "cert": "你申请的证书",
            "key": "证书的key",
            "key_password": "",
            "cipher": "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384",
            "cipher_tls13": "TLS_AES_128_GCM_SHA256:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_256_GCM_SHA384",
            "prefer_server_cipher": true,
            "alpn": [
                "http/1.1"
            ],
            "alpn_port_override": {
                "h2": 81
            },
            "reuse_session": true,
            "session_ticket": false,
            "session_timeout": 600,
            "plain_http_response": "",
            "curves": "",
            "dhparam": ""
        },
        "tcp": {
            "prefer_ipv4": false,
            "no_delay": true,
            "keep_alive": true,
            "reuse_port": false,
            "fast_open": false,
            "fast_open_qlen": 20
        },
        "mysql": {
            "enabled": false,
            "server_addr": "127.0.0.1",
            "server_port": 3306,
            "database": "trojan",
            "username": "trojan",
            "password": "",
            "key": "",
            "cert": "",
            "ca": ""
        }
    }

 _trojan_ 的配置修改完之后，启动 _trojan_ 服务即可。

    trojan /usr/local/etc/trojan/config.json &

 接下来是 _trojan_ 客户端，有很多客户端支持 _trojan_ 协议，这里笔者使用的是[Project V](https://www.v2ray.com/en/awesome/tools.html)的客户端，它既支持 _Project V_ 的协议，也支持 _trojan_ 的协议，配置如下图。

<Image img={require('./asserts/set-up-site6.png')} alt="trojan客户端" />

 最后就是在 _Chrome_ 上安装大名鼎鼎的 _SwitchyOmega_ 插件，_SwitchyOmega_ 目前不能直接从 _Chrome_ 的应用商店安装了，即使手动下载 _crx_ 文件，拖动到 _Chrome_ 里面也会报错。需用 _zip_ 解压工具对 _crx_ 文件进行解压，得到一个插件目录，打开 _Chrome_ 的开发者模式进行加载即可。

<Image img={require('./asserts/set-up-site5.png')} alt="打开开发者模式" />

 现在的网络请求是这样中转的：_Chrome_ → _trojan_ 客户端 → _trojan_ 服务端 → 你想访问的网站。当然 _trojan_ 客户端会过滤掉未被屏蔽的网站，这样速度会快很多。

 接着介绍一下，_SwitchyOmega_ 如何配置。点击 _SwitchyOmega_ 插件，进入选项界面，新建一个情景模式，参考下图进行配置。如果不知道 _trojan_ 客户端监听的端口号，可以在 _trojan_ 客户端主界面的左下角找到。

<Image img={require('./asserts/set-up-site7.png')} alt="SwitchyOmega配置" />

<Image img={require('./asserts/set-up-site8.png')} alt="trojan客户端端口" />


### 8. 搭建站点

 市面上建站的开源框架非常的多，有[Hexo](https://hexo.io/docs/writing.html)、[Jekyll](https://github.com/jekyll/jekyll)、[WordPress](https://wordpress.com/)等，本站点使用的是 _Facebook_ 开源的[docusaurus](https://docusaurus.io/)，有兴趣的朋友可以移步到[docusaurus使用全攻略](/docs/docusaurus/docusaurus-intro)，学习如何使用 _docusaurus_ 搭建站点。

### 9. 发布站点

 上面提到的建站框架支持生成静态站点，编译之后放到 _nginx_ 对应的目录下即可。以 _docusaurus_ 为例，执行 _npm run build_ 之后，在项目根目录下会生成一个 _build_ 文件夹，通过上面介绍的 _WinScp_ 工具非常方便的拷贝到服务器上。

<Image img={require('./asserts/set-up-site9.png')} alt="发布站点" />

 对应站点的 _nginx_ 文件也需求进行修改，修改完成后，重启 _nginx_。

    server {
            server_name linlan.xyz;
            listen 80;
            root /root/build/; 拷贝到服务器的目录
            rewrite  ^/$  https://$server_name/index.html permanent;
    }

### 10. 接入Google Search和Google gtag

 对于一个站点，接入 _Google Search_ 和 _Google gtag_ 是必不可少的。_Google Search_ 可以让你的站点出现在 _Google_ 的搜索结果中，_Google gtag_ 可以记录你的页面访问情况。

#### 10.1 Google Search接入

 登录到[Google Search](https://search.google.com/)之后，点击左上角的添加新资源，会跳出一个对话框。选择后面的 _URL_ 的方式，这个时候会提示验证站点。验证的方式非常简单，下载一个 _html_ 文件，将这个文件放到你站点根目录下（在本文的例子中，就是放在 _build_ 目录下）即可。因为从本地拷贝 _build_ 文件夹到服务器上时，可能会覆盖掉服务器上的 _build_ 目录，导致这个 _html_ 文件丢失，所以建议将这个 _html_ 文件直接打包到你的站点文件里。

<Image img={require('./asserts/set-up-site10.png')} alt="添加新资源" />

 搜索引擎使用 _sitemap_ 来发现站点里面的页面。建站框架提供了 _sitemap_ 生成工具，默认会放到站点的根目录下。例如：_<https://linlan.xyz/sitemap.xml>_。

<Image img={require('./asserts/set-up-site11.png')} alt="添加sitemap" />

#### 10.2 添加robots.txt文件

 _robots.txt_ 用来告诉 _Google_ 等搜索引擎本站点哪些内容可以爬取，哪些内容不能爬取。 该文件的格式如下，和验证站点一样，将 _robots.txt_ 放在 _static_ 目录下，打包的时候就会自动打包到 _build_ 的根目录下。

    User-agent: *
    Disallow: /includes/

    User-agent: Googlebot
    Allow: /includes/

    Sitemap: https://example.com/sitemap.xml

#### 10.3 Google gtag接入

 登录到[Google analytics](https://analytics.google.com/)，点击左下角的齿轮，进入设置页面。如果没有账户，先创建账户，接着创建资源，按照页面的指引下一步即可。最后得到一个 _gtag id_。


<Image img={require('./asserts/set-up-site12.png')} alt="添加gtag" />

 _gtag_ 实际对应一段 _js_ 代码，将这段代码放在站点的 _head_ 标签里面即可，下面代码是例子。拷贝时记得将里面的 _gtag id_ 换成你申请的 _gtag id_。有些框架（例如 _docusaurus_、_hexo_ 等）集成了 _gtag_，只需要在配置文件里面修改 _gtag id_ 即可。

    <!-- Global site tag (gtag.js) - Google Analytics -->
    <script async src="https://www.googletagmanager.com/gtag/js?id=G-0QFZJ5DY3B"></script>
    <script>
      window.dataLayer = window.dataLayer || [];
      function gtag(){dataLayer.push(arguments);}
      gtag('js', new Date());

      gtag('config', 'G-0QFZJ5DY3B');
    </script>

 至此，你也拥有了自己的站点了，接下来就尽情的创作吧！

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
