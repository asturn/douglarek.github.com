---
layout: post
title: Jenkins 插件开发初步
category : ci
tags : [jenkins]
---
{% include JB/setup %}

1. 准备工作
========

## 环境变量设置

* 确保有Maven（jenkins使用maven构建）

* JDK 1.6 或者以上（jenkins使用java编写）

*注意：*如果第一次使用maven，请确保Maven可以联网下载相关依赖：-），如果使用代理，请参考[这篇文章](http://douglarek.github.com/ci/2012/07/13/maven-proxy/)的做法；另外Maven 版本需要在2.0.9以上，我使用的3，因为2在我的openSuse x86_64上遇到一些莫名的错误。

## 使用较短插件名称

在maven的工作目录下，这里使用linux下示例，编辑~/.m2/settings.xml（如果没有，新建一个）：

    <settings>
      <pluginGroups>
        <pluginGroup>org.jenkins-ci.tools</pluginGroup>
      </pluginGroups>

    <profiles>
    <!-- Give access to Jenkins plugins -->
        <profile>
          <id>jenkins</id>
          <activation>
            <activeByDefault>true</activeByDefault>i<!-- change this to false, if you don't like to have it on per default -->
          </activation>
          <repositories>
            <repository>
              <id>repo.jenkins-ci.org</id>
              <url>http://repo.jenkins-ci.org/public/</url>
            </repository>
          </repositories>
          <pluginRepositories>
            <pluginRepository>
              <id>repo.jenkins-ci.org</id>
              <url>http://repo.jenkins-ci.org/public/</url>
            </pluginRepository>
          </pluginRepositories>
        </profile>
      </profiles>
    </settings>

这样会使用hpi:create代替org.jenkins-ci.tools:maven-hpi-plugin:1.61:create这个较长的名称。
2. 创建一个新的插件
==================

使用下面的maven命令：

    $ mvn -cpu hpi:create

这条命令最后会询问要建立的插件的名称（artifactId),然后建立一个以这个artifactId命名的目录。

如果命令执行成功，则会建立一个新的模板，确保无误，使用下面的命令：

    $ cd newly-created-directory
	$ mvn package

以上的命令中-cpu是maven的一个选项，意思是在执行命令前，将maven的插件更新到最新，hpi：这是jenkins插件的后缀，一般的jenkins的插件被打包成hpi形式。create：创建一个自定义的名称目录。

*注意：*这条命令你未必能执行成功（我使用的是git克隆的源码，切换到tag：1.474），因为通过hpi:create生成的这个目录下的pom.xml中链接有问题：

    <!-- get every artifact through maven.glassfish.org, which proxies all the artifacts that we need -->
    <repositories>
        <repository>
	        <id>m.g.o-public</id>
			<url>http://maven.glassfish.org/content/groups/public/</url>
	    </repository>
	</repositories>
	<pluginRepositories>
	    <pluginRepository>
			<id>m.g.o-public</id>
			<url>http://maven.glassfish.org/content/groups/public/</url>
	    </pluginRepository>
	</pluginRepositories>

上面的[http://maven.glassfish.org/content/groups/public/]已经不可用，所以需要修改为jenkins自己的public仓库，url为：

    http://repo.jenkins-ci.org/public/

3. 构建一个插件
==============

使用以下的命令执行构建：

    $ mvn install

然后会在这个新的目录下产生一个./target/pluginname.hpi的文件，然后部署到jenkins上即可。
