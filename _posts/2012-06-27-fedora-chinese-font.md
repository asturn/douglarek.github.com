---
layout: post
title: 解决fedora 17 英文环境中文字体问题
category : linux
tags : [fedora]
---
{% include JB/setup %}

自从 Fedora 17 发布之后, 便开始装一个玩玩(之前也升级过, 效果不好), 但是不知道是哪个蛋疼的维护者把Fedora 16原本平凡无奇的字体搞没了(真心狠死人啊), 新字体仿佛回到了某个起点, 烂的一比(我说的是中文字体)；

没办法, 只好使用英文界面了, 但是蛋疼的问题接踵而至, 中文字体大小不一, 恼死人啊!

没办法, 查到了Fedora开发者中文组邮件列表, 找到了解决方案:

在～/.bash_profile文件下加入：
{% highlight bash %}
export PANGO_LANGUAGE=en:zh_CN
{% endhighlight%}
ok, 生效即可.
