---
layout: post
title: Hotot：A cool twitter client in Linux
category : chat
tags : [fedora, hotot]
---
{% include JB/setup %}

周日在家，闲来无事，准备在linux上安装个twitter客户端玩玩，说实话，现在Linux下的Gwibber界面丑的要命，真不想用，然后就在Google上搜索，不经意间一个叫作Hotot（中文傲兔）吸引了我，她是中国人的作品（少见的小而精悍的东东，在这里需要向作者道歉，我以为是外国人做的，但是不可否认的是它抄袭了Turpial的创意），然后在我的fedora 16上安装了一下，下面是安装的命令（root下）：
{% highlight bash %}
yum install hotot
{% endhighlight %}
ok，来张小图：

![Alt text](/images/hotot.png)

但是这个版本的Hotot是0.9.3版的，与最新的（0.9.8）相比不够好，但是界面差距不大，功能增加的不少，怎么整？果断building from source啊！Hotot是一个open source，源码托管在Github（这个星球上最著名的git托管网站，没有之一，现在的程序员没有一个Github帐号，都不好意思在这个星球混（言外之意，火星了呗）），下载源码：
{% highlight bash %}
git clone git://github.com/shellex/Hotot.git
{% endhighlight %}
如果你想贡献代码，首先fork一个Hotot仓库，然后发Pull Requests即可（这个怎样做，不多说，Github上说的不能再清楚了）。下面说怎么编译？关于怎么编译在Hotot这个项目的Wiki上说的不是很清楚，因为Hotot使用cmake编译工具，而该Wiki说了依赖的库（参见此处：[https://github.com/shellex/Hotot/blob/master/README.md](https://github.com/shellex/Hotot/bl\)），但是没说怎么定制编译，只是简短的提示了一下，这里说一下自己摸索的： 
{% highlight bash %}
### 我的桌面是Gnome3，所以不编译KDE版本（QT版本）；如果使用默认选项，则编译gtk with gir，qt版本，我这里只编译gtk版（不需要安装qt依赖，省下很多的事情）。
mkdir build && cd build
### 关闭qt编译
cmake -DWITH_QT=off ..
make
### 需要root
make install
{% endhighlight %}
ok，搞定了，下面打开程序即可，如果是天朝用户（请别忘记使用代理，这个小东东，有这个选项，很cool），good luck！
