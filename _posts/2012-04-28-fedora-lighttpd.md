---
layout: post
title: Fedora 下安装 lighttpd
category : tool
tags : [fedora, lighttpd]
---
{% include JB/setup %}

在fedora下装个lighttpd其实挺简单的，这里不过是怕自己忘，记下来吧，安装：

    sudo yum install lighttpd
    # 如果sudo不可用，则使用su
    su -c 'yum install lighttpd'

安装服务: 

    sudo chkconfig --levels 235 lighttpd on

启动它： 

    sudo service lighttpd start

ok，搞定！

