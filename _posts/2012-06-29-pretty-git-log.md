---
layout: post
title: 漂亮的Git log
category : scm
tags : [git]
---
{% include JB/setup %}

看到coolshell[这篇文章](http://coolshell.cn/articles/7755.html)(实际翻译自这个[blog](http://garmoncheg.blogspot.com/2012/06/pretty-git-log.html)）显示pretty git log的方法，的确不错，但是原文作者听不进去评论者的话，所以这个版本不是最好的版本，译者也没有领悟到这一点（只顾翻译了），ok，以我对Git的了解，记录一下；

我们知道使用git log 查看仓库的log是非常丑陋的，怎么个丑陋法，程序猿的世界无法只有黑白，那样太单调：

![Alt text](/images/git-log.png)

看看作者的改进，有点颜色了：

![Alt text](/images/pretty-git-log.png)

作者实际上定义了一下format形式，做了如下的使用：

    git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --

然后作者觉得如果每次都敲这么长，神也记不住，所以使用了Git的别名机制：

    git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --"

然后使用方法：

    git lg

本来这样就近乎完美了，但是看了一下原文作者的blog评论，一些童鞋提出了复用这个format的方法：

    git config --global pretty.graph '%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset'

这样相当于定义了一种新的pretty：graph，然后使用别名：

    git config --global alias.lg 'log --pretty=graph --graph --abbrev-commit --'

这样有什么好处呢？复用pretty，比如我显示HEAD的详细提交信息：

    git show --pretty=graph

国外的牛人还是很多的，评论的人都很强大：-）
