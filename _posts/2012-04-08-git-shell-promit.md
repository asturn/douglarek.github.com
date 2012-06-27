---
layout: post
title: 在 Shell 提示符中显示 Git 分支以及当前的仓库状态
category : program
tags : [shell, git]
---
{% include JB/setup %}

git是个好工具，我们使用的时候当然希望在shell提示符显示git分支的相关情况，方法有那么两种，下面一一介绍：

## 非git官方脚本

来自Aaron Crane，实现方式比较简单，具体的分析可以点击链接查看该大神的网站，下面简单描述一下：

读取git分支实际是很简单的，下面的命令轻松获得：

    git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'

Aaron Crane 的实现方式在此不再赘述，下面是一个简化的版本：

    function parse_git_dirty {
    [[ $(git status 2> /dev/null | tail -n1) != "nothing to commit (working directory clean)" ]] && echo "*"
    }

    function parse_git_branch {
    git branch --no-color 2> /dev/null | sed -e '/^[^*]/d' -e "s/* \(.*\)/(\1$(parse_git_dirty))/"
    }

    PS1="\[\033[1;31m\]\u \[\033[1;34m\]\$(/usr/bin/tty | /bin/sed -e 's:/dev/::'):\W\[\033[1;31m\]\$(parse_git_branch)\[\033[1;35m\] -> \[\033[0m\]"

以上只是很简单的展示git分支的状态，实际的状态是很复杂的，包括3个部分的数据的比对：工作区、暂存区、git仓库。parse_git_dirty方法中的“nothi　to commit (working directory clean)”是根据git status得出的，从git1.7.10开始，git则提供了多语言提示，所以如果你的linux版本为中文简体，则改为“无须提交（干净的工作区）”．

## git官方脚本

通过git-complete.bash，该脚本可以通过各个发行版安装，也可以从git源码拷贝，下载git源码的地址：

    git clone git://git.kernel.org/pub/scm/git/git.git

然后拷贝contrib/completion/git-completion.bash到～下，重命名为.git-completion.bash，然后加入.bash中：

    source ～/.git-completion.bash

接下来根据此官方脚本配置，下面的一系列的命令皆来自git-completion.bash的说明，不再赘述：

    # 配置相关变量来显示不同的git仓库的状态，其中的“1”可以为任意的非空值
    export GIT_PS1_SHOWDIRTYSTATE=1
    export GIT_PS1_SHOWSTASHSTATE=1
    export GIT_PS1_SHOWUNTRACKEDFILES=1
    export GIT_PS1_SHOWUPSTREAM="verbose git svn"

    PS1='\[\033[1;31m\]\u@\h \[\033[1;34m\]\W\[\033[1;31m\]$(__git_ps1 " (%s)")\[\033[1;35m\] -> \[\033[0m\]'
---
总结，很明显第2种方式是推荐的，实现起来效率更高，而且git仓库的状态很全面：P
