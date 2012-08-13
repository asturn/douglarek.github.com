---
layout: post
title: "openSUSE 12.2 x86_64 上 Android 模拟器启动问题"
category: develop
tags: [android]
---
{% include JB/setup %}

有些日子没搞android应用开发了，今天手痒了，随便跑起了模拟器，奇怪的现象出现了：

    SDL init failure, reason is: No available video device

说的很明了，SDL初始化失败！SDL是什么玩意呢？（Simple DirectMedia Layer，是一套开放源码的跨平台多媒体开发库，使用C语言写成。）由于我使用的是64bit的openSUSE，所以默认一些图形相关的一些32bit库没装，具体的说就是这两个：libXrandr2-32bit以及libX11-6-32bit，然后安装即可。在终端执行下面的命令：

    suod zypper in libXrandr2-32bit

由于libX11与libXrandr存在依赖关系，所以会顺便将libX11装上。

最后再次启动模拟器，ok。
