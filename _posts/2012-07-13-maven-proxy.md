---
layout: post
title: Maven 代理设置
category : ci
tags : [maven, java]
---
{% include JB/setup %}

有时候公司（eg：intel）出于安全考虑，要求使用安全认证的代理上网，正所谓在正规的公司，代理无处不在：—）

实际上呢，网络上有很多设置maven代理的方法，但是大多都是错的，不知道为什么以讹传讹（或许这是网络的通病）；

为什么设置maven代理？我们知道大多数使用maven管理的项目都会有pom.xml文件，有些项目包含子项目，所以会从网络下载pom.xml文件来处理相关依赖，这个需求是我在openSuse下编译jenkins遇到的下载失败得到的，所以下面设置代理的方法，很简单（简单的掉渣）；

下面的一些说明均来自openSuse的相关操作，其他发行版抑或也可以；

首先配置好maven，将其bin路径放入path中，然后配置$MAVEN_HOME/conf/下的settings.xml即可，搜索proxy标签，然后设置相关代理：

    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>

不要忘了将proxy标签的-->注释去掉，将上面的代理设置换成自己的即可：-）
