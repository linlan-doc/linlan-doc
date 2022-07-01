---
sidebar_position: 1
title: 搭建站点全攻略（多图）
keywords: ['站点搭建','域名购买','vpn搭建']
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

:::note

说明：因中国大陆审查严格，故本攻略仅针对海外建站。

:::

### 准备工作

 购买海外的云服务产品可能需要一张支持美元的信用卡，所以需要提前申请。

### 1. 服务器选择

 海外的服务器提供商主要有：_Google Cloud_、_Amazon AWS_、_Vultr_、_Digital Ocean_等。_Google Cloud_和_Amazon AWS_都提供免费试用的服务，但这两家产品都偏贵，而且网络传输并不免费，在试用的时候需要格外小心。

 个人建站推荐使用_Vultr_和_Digital Ocean_，这两家最便宜的服务器只需要_5$/月_。笔者使用比较多的是[_Vultr_](https://www.vultr.com/?ref=8912579)，新用户注册可以领取_100$_的体验券，但这张体验券有效期只有_1_个月，即最多白嫖一个月。

 在选择将服务器部署到哪个国家之前，需要对该国的网络进行测速。_ping test_的工具很多，例如[站长之家](https://ping.chinaz.com/)的_ping_工具。云服务器厂商提供了测试的_ip_，在_Google_上搜索_xxx ping test_就能搜到云厂商的测速页面，例如_Vultr_的[ping test](https://sgp-ping.vultr.com/)。你可以选择一个国家，获取到测试_ip_，然后用_ping_工具进行测试。从笔者使用情况看_Vultr_韩国的服务器比较好，国内的延迟大约_150ms_。

 接下来需要选择操作系统镜像，选择_Clound Compute_，这里面的套餐最低_5$/月_，同时包含了_1000G_的流量，个人站点足够了。笔者使用的是_Ubuntu_的镜像，因此本文基于_Ubuntu_进行编写的。
![镜像选择](./asserts/set-up-site2.png)

### 2. 登录服务器

 云服务器厂商提供了登录云服务器的工具，以_Vultr_为例，进入服务器实例详情，点击右上角_View Console_即可登录到服务器。当然你也可以使用_ssh_命令进行登录，在实例详情页面可以找到_ip_地址、端口以及密码。
![登录到服务器](./asserts/set-up-site1.png)

 如果是_windows_用户，可以安装_WinScp_这款工具，拷贝文件非常方便，同时也可以集成_putty_。

### 3. 安装nginx

 _nginx_是一款风靡全球的反向代理服务器，个人站点多数是静态站点，用_nginx_作为服务器非常方便。

    sudo apt-get install nginx

 安装完成之后，需要配置域名。以_linlan.xyz_为例，在_/etc/nginx/conf.d_目录下，新建_linlan.conf_，这个配置文件的内容非常简单：指定了端口号和域名。

    server {
            server_name linlan.xyz;
            listen 80;
    }

 接下来需要重启一下_nginx_，让_linlan.conf_配置生效。

    nginx -t //测试一下配置是否正常
    nginx -s reload //重启服务

### 4. 购买域名

 域名是我们站点的名字，海外有一些知名的域名供应商，例如[Godaddy](https://www.godaddy.com)、[Domain](https://www.domain.com/)、[NameSilo](https://www.namesilo.com/?rid=2ddf330gz)，推荐使用_NameSilo_，
_NameSilo_首年会有优惠，续费也没那么多套路，几十块一年，_DNS_解析不需要额外收费，并且支持支付宝付款。

#### 4.1 配置DNS

 购买域名后，需要将域名指向云服务器。在_NameSilo_的首页，点击右上角的_Manage Domain_（如下图)，进入到域名列表，找到你想要绑定的域名，点击_Manage DNS for this domain_，进入_DNS_配置页面。

![管理域名](./asserts/set-up-site3.png)

 以本站为例，_linlan.xyz_是一个二级域名，如果要让_linlan.xyz_指向购买的服务器，需要新建一个类型_A_的域名解析记录。_HostName_选_@.linlan.xyz_，_IP Address_填写云服务器对外的_IP_地址。_NameSilo_生效的时间比较长，需要等一会域名解析才能生效，使用_ping_命令可查看域名解析是否已经生效。

![配置解析记录](./asserts/set-up-site4.png)

### 5. 检查服务器http和https端口

 域名解析生效之后，在浏览器输入购买的域名，就能够看到_nginx_的欢迎页面了。如果_ping_和_ssh_都正常，但是页面无法正常打开，这时需要检查服务器是否允许访问_80(http)_和_443(https)_端口。如果端口没有打开，可以通过下面命令开启，开启之后再测试一下页面是否能够正常访问。

    ufw status verbose //查看防火墙状态，确认端口是不是关闭
    sudo ufw allow 80 //打开80端口
    sudo ufw allow 443 //打开443端口

### 6. HTTPS证书申请

 _https_已经是站点的标配了，_Let’s Encrypt_提供免费的_https_证书，可以为你的站点申请一个。

#### 6.1 安装_Encrypt Client_

<Tabs groupId="operating-systems">
  <TabItem value="python2" label="python2"><code>sudo apt-get install certbot <br/>apt-get install python-certbot-nginx</code></TabItem>
  <TabItem value="python3" label="python3"><code>sudo apt-get install certbot<br/>apt-get install python3-certbot-nginx</code></TabItem>
</Tabs>

#### 6.2 nginx配置（见章节3）

#### 6.3 生成证书

    sudo certbot --nginx -d example.com

 上述命令执行完成之后，你可以前往_/etc/nginx/conf.d_文件夹下，查看站点的配置。你会发现配置的内容被_cerbot_修改过，增加了_https_的内容。打开浏览器，使用_https_请求站点，确定_https_证书是否正确安装。

#### 6.4 刷新证书

 _Let's Encrypt_生成的证书有_90_的有效期，所以需要定时的刷新。

-   打开_crontab_

        crontab -e

-   新建自动刷新命令

        0 12 * * * /usr/bin/certbot renew --quiet

### 7. trojan安装

 _VPN_的协议有很多，_SS_、_Project V_和_Trojan_，笔者比较推荐_Trojan_，相对稳定，防干扰的能力出色。

    sudo bash -c "$(curl -fsSL https://raw.githubusercontent.com/trojan-gfw/trojan-quickstart/master/trojan-quickstart.sh)"

 _trojan_会监听_443_端口，如果不是_trojan_协议的流量，_trojan_会将请求转发到_80_端口，故_nginx_需关掉_https_的端口，只需将章节_6_中_cerbot_增加的_https_内容去掉即可，但需要记录证书和_key_的路径，这两个配置项在配置_trojan_的时候需要用到。

    server {
            server_name linlan.xyz;
            listen 80;
    }

 修改完_nginx_的配置文件之后，重启一下_nginx_。打开_trojan_的配置文件，_Ubuntu_下路径为_/usr/local/etc/trojan/config.json_，修改三个地方：密码，_cert_和_key_。

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

 _trojan_的配置修改完之后，启动_trojan_服务即可。

    trojan /usr/local/etc/trojan/config.json &

 接下来是_trojan_客户端，有很多客户端支持_trojan_协议，这里笔者使用的是[Project V](https://www.v2ray.com/en/awesome/tools.html)的客户端，它既支持_Project V_的协议，也支持_trojan_的协议，配置如下图。
![trojan客户端](./asserts/set-up-site6.png)

 最后就是在_Chrome_上安装大名鼎鼎的_SwitchyOmega_插件，_SwitchyOmega_目前不能直接从_Chrome_的应用商店安装了，即使手动下载_crx_文件，拖动到_Chrome_里面也会报错。需用_zip_解压工具对_crx_文件进行解压，得到一个插件目录，打开_Chrome_的开发者模式进行加载即可。
![打开开发者模式](./asserts/set-up-site5.png)

 现在的网络请求是这样中转的：_Chrome_ ->  _trojan_客户端 ->  _trojan_服务端 -> 你想访问的网站。当然_trojan_客户端会过滤掉未被屏蔽的网站，这样速度会快很多。

 接着介绍一下，_SwitchyOmega_如何配置。点击_SwitchyOmega_插件，进入选项界面，新建一个情景模式，参考下图进行配置。如果不知道_trojan_客户端监听的端口号，可以在_trojan_客户端主界面的左下角找到。
![SwitchyOmega配置](./asserts/set-up-site7.png)
![trojan客户端端口](./asserts/set-up-site8.png)

### 8. 搭建站点

 市面上建站的开源框架非常的多，有[Hexo](https://hexo.io/docs/writing.html)、[Jekyll](https://github.com/jekyll/jekyll)、[WordPress](https://wordpress.com/)等，本站点使用的是_Facebook_开源的[docusaurus](https://docusaurus.io/)，有兴趣的朋友可以移步到[docusaurus使用全攻略](/docs/docusaurus/docusaurus-intro)，学习如何使用_docusaurus_搭建站点。

### 9. 发布站点

 上面提到的建站框架支持生成静态站点，编译之后放到_nginx_对应的目录下即可。以_docusaurus_为例，执行_npm run build_之后，在项目根目录下会生成一个_build_文件夹，通过上面介绍的_WinScp_工具非常方便的拷贝到服务器上。
![发布站点](./asserts/set-up-site9.png)

 对应站点的_nginx_文件也需求进行修改，修改完成后，重启_nginx_。

    server {
            server_name linlan.xyz;
            listen 80;
            root /root/build/; 拷贝到服务器的目录
            rewrite  ^/$  https://$server_name/index.html permanent;
    }

### 10. 接入Google Search和Google gtag

 对于一个站点，接入_Google Search_和_Google gtag_是必不可少的。_Google Search_可以让你的站点出现在_Google_的搜索结果中，_Google gtag_可以记录你的页面访问情况。

#### 10.1 Google Search接入

 登录到[Google Search](https://search.google.com/)之后，点击左上角的添加新资源，会跳出一个对话框。选择后面的_URL_的方式，这个时候会提示验证站点。验证的方式非常简单，下载一个_html_文件，将这个文件放到你站点根目录下（在本文的例子中，就是放在_build_目录下）即可。因为从本地拷贝_build_文件夹到服务器上时，可能会覆盖掉服务器上的_build_目录，导致这个_html_文件丢失，所以建议将这个_html_文件直接打包到你的站点文件里。

![添加新资源](./asserts/set-up-site10.png)

 搜索引擎使用_sitemap_来发现站点里面的页面。建站框架提供了_sitemap_生成工具，默认会放到站点的根目录下。例如：_https://linlan.xyz/sitemap.xml_。

![添加sitemap](./asserts/set-up-site11.png)

#### 10.2 Google gtag接入

 登录到[Google analytics](https://analytics.google.com/)，点击左下角的齿轮，进入设置页面。如果没有账户，先创建账户，接着创建资源，按照页面的指引下一步即可。最后得到一个_gtag id_。

![添加sitemap](./asserts/set-up-site12.png)

 _gtag_实际对应一段_js_代码，将这段代码放在站点的_head_标签里面即可，下面代码是例子。拷贝时记得将里面的_gtag id_换成你申请的_gtag id_。有些框架（例如_docusaurus_、_hexo_等）集成了_gtag_，只需要在配置文件里面修改_gtag id_即可。

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
