---
layout: post
title: 解决Jenkins插件git-plugin编译错误
category : ci
tags : [jenkins]
---
{% include JB/setup %}

对于一个做Java项目的人来说，Maven和其他的构建工具一样都有与生俱来的便捷性，但是不可否认它的缺点让我也很恼火，太多的依赖需要联网（主要是根据pom.xml）下载，而这些个pom文件下载的大多是一些不同个源的依赖，某个坏了，整个编译就挂了；

最近做jenkins相关的开发，主要是git以及gerrit这块，想看看git-plugin的实现，然后就想，先按步骤编译一个自己的出来，于是按惯例执行下面的命令：

    mvn package

不料出现了下面的驴腿不对马嘴的错误：


	[INFO] Scanning for projects...
	[ERROR] The build could not read 1 project -> [Help 1]
	[ERROR]
	[ERROR]   The project org.jenkinsci.plugins:git:1.1.21 (/work/ci/plugins/git-plugin/pom.xml) has 1 error
	[ERROR]     Non-resolvable parent POM: Failure to find org.jenkins-ci.plugins:plugin:pom:1.424 in http://download.eclipse.org/jgit/maven was cached in the local repository, resolution will not be reattempted until the update interval of jgit-repository has elapsed or updates are forced and 'parent.relativePath' points at wrong local POM @ line 3, column 11 -> [Help 2]
	[ERROR]
	[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
	[ERROR] Re-run Maven using the -X switch to enable full debug logging.
	[ERROR]
	[ERROR] For more information about the errors and possible solutions, please read the following articles:
	[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
	[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException

意思大体说的明了，缺少一个org.jenkinsci.plugins:git的model，但是对于我门这些初学者来说，我咋知道这个model从哪找呢？你maven不是自动构建工具吗？怎么还问我有木有呢？

没办法了，不能让这个问题拦住，于是去Google Groups的jenkins-dev论坛开了个帖子专侯大神，这个问题以及解决方案的帖子（可能需要FQ）在这：

    https://groups.google.com/forum/?fromgroups#!topic/jenkinsci-dev/tvgvHSv7UE4

其中一个哥们给出了解决问题的一个patch，这是一个git补丁，在此下载：

    https://issues.jenkins-ci.org/secure/attachment/22111/0001-Add-Jenkins-repo-to-pom.xml.patch

这个补丁的完整内容如下，大致就是添加了jenkins的公共插件仓库：

    From 381ebeece06015a69c49a289b908ffd0582d9cb2 Mon Sep 17 00:00:00 2001
    From: Bryan Jacobs <bjacobs@woti.com>
    Date: Mon, 16 Jul 2012 11:24:25 -0400
    Subject: [PATCH] Add Jenkins repo to pom.xml

    Without this repository, some of the dependencies cannot be fetched on
    an unconfigured Maven instance.
    ---
     pom.xml | 6 ++++++
     1 个文件被修改，插入 6 行(+)

    diff --git a/pom.xml b/pom.xml
    index 95bead1..131470e 100644
    --- a/pom.xml
    +++ b/pom.xml
    @@ -64,6 +64,12 @@
           <name>guice maven</name>
           <url>http://guice-maven.googlecode.com/svn/trunk</url>
         </repository>
    +
    +    <repository>
    +      <id>jenkins</id>
    +      <name>jenkins repository</name>
    +      <url>http://repo.jenkins-ci.org/public/</url>
    +    </repository>
       </repositories>

        <developers>
    --
    1.7.11.2

然后当然是ok了，-:)，但是我的余怒未消，就这样一个小的插件项目导入eclipse一看，有100多个依赖的jar？我去，写pom.xml的时候，那叫一个简单，了了数行代码，但是编译却需要下载这么多依赖，真是很无语的说，与以前做Buildbot的时候真是截然不同；Maven，但愿我能适应你的工作方式，暂时给你跪了。

**7.23 最新更新**

git-plugin插件的最新提交已经解决这个问题，因此这个问题已经不复存在：-）

    https://github.com/jenkinsci/git-plugin/commit/24254f4223772509f3487e3fbcc29a03346c8c8b
