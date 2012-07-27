---
layout: post
title: "openSUSE 12.x 设置代理"
category: linux
tags: [opensuse]
---
{% include JB/setup %}

最近出差的过程中，使用的电脑装有openSUSE 12.1系统,想联网、更新系统等都需要设置代理，浏览器（比如火狐）的代理设置很简单，有专门的可视化选项；想更新openSUSE系统怎么办呢？就需要设置zypper的代理，怎么设置？这要用到系统代理。

我们需要找到/etc/sysconfig/proxy这个文件，然后添加自己的http_proxy,https_proxy,以及no_proxy：

    Network/Proxy

    yesno
    no
    ## Config:      kde,profiles
    #
    # Enable a generation of the proxy settings to the profile.
    # This setting allows to turn the proxy on and off while
    # preserving the particular proxy setup.
    #
    PROXY_ENABLED="yes"

    string
    ""
    #
    # Some programs (e.g. lynx, arena and wget) support proxies, if set in
    # the environment.  SuSEconfig can add these environment variables to
    # /etc/SuSEconfig/* (sourced by /etc/profile etc.) -
    # See http://portal.suse.com/sdb/en/1998/01/lynx_proxy.html for more details.
    # Example: HTTP_PROXY="http://proxy.provider.de:3128/"
    HTTP_PROXY="自己的代理IP"

    string
    ""
    #
    # Some programs (e.g. lynx, arena and wget) support proxies, if set in
    # the environment.  SuSEconfig can add these environment variables to
    # /etc/SuSEconfig/* (sourced by /etc/profile etc.) -
    # this setting is for https connections
    HTTPS_PROXY="自己的代理IP"

    string
    ""
    #
    # Example: FTP_PROXY="http://proxy.provider.de:3128/"
    #
    FTP_PROXY=""

    string
    ""
    #
    # Example: GOPHER_PROXY="http://proxy.provider.de:3128/"
    #
    GOPHER_PROXY=""

    string(localhost)
    localhost
    #
    # Example: NO_PROXY="www.me.de, do.main, localhost"
    #
    NO_PROXY="localhost, 127.0.0.1"
    ## Default:## Type:## Default:## Type:## Default:## Type:## Default:## Type:## Default:## Type:## Default:## Type:## Description:## Path:

这样可能还不完美，因为openSUSE源的类型大致有3种，而且下载软件包的协议不尽相同，其中有一种常用的类型（不记得哪种了）需要使用到一种aria2的东东，这哥们不走系统代理，所以需要单设代理！ok，也很简单，需要切换到root下，然后在其主目录下建立.aria2/aria2.conf这样一个文件，内容如下：

    http_proxy=自己的代理IP

然后复制一份同样的目录文件到自己的非root用户下即可:-)
