---
sidebar_position: 3
title: Ubuntu 22.04安装nginx加速插件PageSpeed
keywords: ['网页加速','nginx','PageSpeed','Ubuntu']

toc_max_heading_level: 4
---

import Image from '@theme/IdealImage';

 [PageSpeed](https://developers.google.com/speed/pagespeed/module)是谷歌公司开发的网页加速工具，它提供 _Apache_ 服务器和 _Nginx_ 两个版本，本文以 _Ubuntu 22.04_ 为例介绍如何安装 _PageSpeed_ 插件。

### 1. 自动安装

 _Google_ 提供了自动安装的脚本，不过笔者在实际使用的过程中会报错，所以选择手动安装。

    bash <(curl -f -L -sS https://ngxpagespeed.com/install) \
         --nginx-version latest

### 2. 手动安装

 安装 _PageSpeed_ 插件依赖 _Nginx_ 的源码，所以安装的步骤比较多。

#### 2.1 安装依赖包

 _PageSpeed_ 依赖一些三方包，在安装之前确保依赖的三方包都安装完成。

    apt-get update -y
    apt-get install dpkg-dev build-essential zlib1g-dev libpcre3 git libpcre3-dev unzip -y

#### 2.2 安装nginx

 _PageSpeed_ 是一款 _Nginx_ 插件，所以需要先安装 _Nginx_，已经安装的读者可以跳过。

    apt-get install nginx -y

 确认 _Nginx_ 的版本。

    nginx -v

    nginx version: nginx/1.18.0 (Ubuntu)

#### 2.3 下载nginx源代码

 _Ubuntu 22.04_ 默认安装的 _Nginx_ 版本为1.18.0，所以我们下载1.18.0的源代码。

    wget http://nginx.org/download/nginx-1.18.0.tar.gz

    tar -xvzf nginx-1.18.0.tar.gz

#### 2.4 下载PageSpeed源代码

 _PageSpeed_ 的源代码通过 _git_ 进行管理，将代码拷贝到本地，并切换到最新的稳定分支。

    git clone https://github.com/apache/incubator-pagespeed-ngx.git

    cd incubator-pagespeed-ngx
    git checkout latest-stable

#### 2.5 下载psol

 编译 _PageSpeed_ 需要下载 _psol_ 的包，在`incubator-pagespeed-ngx`目录下，执行`cat PSOL_BINARY_URL`会得到对应 _psol_ 的下载地址，其中`BIT_SIZE_NAME`表示`x64`、`ia32`等。

    https://dl.google.com/dl/page-speed/psol/1.13.35.2-$BIT_SIZE_NAME.tar.gz

 笔者的机器是`x64`，故下载对应的版本。

    wget https://dl.google.com/dl/page-speed/psol/1.13.35.2-x64.tar.gz

    tar -xvzf 1.13.35.2-x64.tar.gz

:::caution

 _Ubuntu 22.04_ 使用1.13.35.2的 _psol_ 会报错。参考[github issue](https://github.com/apache/incubator-pagespeed-ngx/issues/1743)，前往[psol-jammy](http://www.tiredofit.nl/psol-jammy.tar.xz)下载 _psol_。

    ngx_pagespeed.so undefined symbol: pthread_mutex_consistent_np

:::

#### 2.6 编译

 _PageSpeed_ 所需的源代码已经下载完成，接下来就是编译插件。

    cd /root/nginx-1.18.0
    apt-get build-dep nginx
    apt-get install uuid-dev

    ./configure --with-compat --add-dynamic-module=/root/incubator-pagespeed-ngx

    make modules

    cp objs/ngx_pagespeed.so /usr/share/nginx/modules/

#### 2.7 修改nginx配置

 _PageSpeed_ 模块编译完成之后，需要动态加载该模块，打开`/etc/nginx/nginx.conf`，在第一行加上下面内容。

    load_module modules/ngx_pagespeed.so;

 在每个需要开启 _PageSpeed_ 的 _server_ 块配置中加入以下内容。

    pagespeed on;

    # Needs to exist and be writable by nginx.  Use tmpfs for best performance.
    pagespeed FileCachePath /var/ngx_pagespeed_cache;

    # Ensure requests for pagespeed optimized resources go to the pagespeed handler
    # and no extraneous headers get set.
    location ~ "\.pagespeed\.([a-z]\.)?[a-z]{2}\.[^.]{10}\.[^.]+" {
      add_header "" "";
    }
    location ~ "^/pagespeed_static/" { }
    location ~ "^/ngx_pagespeed_beacon$" { }

 _PageSpeed_ 使用`/var/ngx_pagespeed_cache`(可以修改)作为文件缓存的路径，所以需要创建这个文件夹，并赋予`nginx`读写权限。

    mkdir -p /var/ngx_pagespeed_cache

    chown -R www-data:www-data /var/ngx_pagespeed_cache

#### 2.8 重启nginx

 修改完配置，重启配置即可。

    nginx -t
    nginx -s reload

#### 2.9 检查是否生效

 使用`curl`命令检查插件是否生效即可。

    curl -p -I https://linlan.xyz

<Image img={require('./asserts/set-up-site17.png')} alt="pagespeed生效" />

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
