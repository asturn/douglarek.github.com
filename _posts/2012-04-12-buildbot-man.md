---
layout: post
title: Buildbot：持续集成（CI）构建利器
category : ci
tags : [buildbot]
---
{% include JB/setup %}

BuildBot是一个系统的自动化编译/测试周期最需要的软件，以验证代码的变化。通过自动重建和测试每次发生了变化的东西，在建设迅速查明之前，减少不必要的失败。有警告计数， 图像大小，编译时间，以及其他参数，随着时间的推移可进行跟踪，让信息变得更明显，因此更容易得到改善。

BuildBot使用python编写，使用到了python Twisted网络包，总体来说，功能十分强大。

功能和特点:

> 运行于各种各样的平台

> 编译过程：使用C,Python等任何语言处理

> 最小的环境要求：Python和网线

> 通过网页，电子邮件，IRC等方式工作

> 轨道在进步的基础，提供估计完工时间

> 通过类进行灵活的配置

> 新工作方式的调试工具

最近公司里使用BuildBot自动集成，顺便恶补了一下python，python还算比较有爱的语言. BuildBot官方网站[http://trac.buildbot.net/](http://trac.buildbot.net/)，作为一个开源项目，文档还是挺全面的，但是不够详细，所以研究的时候还是需要hack源码的. BuildBot托管在Github上，使用git作版本控制工具，所以下载地址如下： 		    
{% highlight bash %}
git clone git://github.com/buildbot/buildbot.git
{% endhighlight %}
由于Buildot大量使用Python Twisted库，所以也可以下载Twisted源码查阅，Twisted官方网站：[http://twistedmatrix.com/trac/](http://twistedmatrix.com/trac/)，源码使用svn作版本控制工具，由于本人对svn不感冒，此处地址从略。天不早了，睡觉，明晚继续！ 
