---
layout: post
title: Set up Gerrit and Jenkins CI Environment
category : ci
tags : [jenkins, gerrit]
---
{% include JB/setup %}

相较于Buildbot的丑陋不堪以及配置艰难（这不是说我不喜欢Buildbot，因为我喜欢文本化的东西，这体现了Unix的一种哲学），Jenkins仿佛是这个世界美好的事物，轻轻一点，键盘一敲（相较于Buildbot的文本化配置，孰优孰劣且不去论），万事俱备，只欠触发。

上面是一些题外话了，当然关于CI以及相关的概念不是本文论述的重点。另外，本文假设你熟悉Gerrit（至少知道干嘛使的），Git，知道Jenkins。

本文打算介绍一下Gerrit 以及 Jenkins的结合，来进行团队开发。

### 搭建Gerrit相关
<p />
为了完成这个Demo，我们首先需要搭建一个开发用的Gerrit服务器，假设搭建完毕，地址为localhost:8081，选择8081是为了避免和Jenkins的8080相冲突（当然Jenkins也可以在启动的时候自定义端口）；为了完成测试，我们需要注册一些用户来完成相关操作（此处略）；另外我们假设还建立了一个名为test的工程。

### 将Jenkins与Gerrit关联
<p />
首先启动Jenkins：

    java -jar jenkins --httpPort=8080

端口默认8080,所以后面的httpPort参数可以省略。

启动完毕之后，我们需要进入“系统管理“里面安装插件，以下两个插件必不可少：Git-plugin、Gerrit-trigger。然后重启Jenkins即可生效。
