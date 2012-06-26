---
layout: post
title: Fedora 删除多余内核
category : linux
tags : [fedora, kernel]
---
{% include JB/setup %}

今天偶然更新Fedora 17系统，出现了一个内核更新的错误，boot空间不足了，怎么办呢？依然抵挡不住新的内核（3.4.2）的诱惑，果断升级啊，肿么办？

先看错误信息：

    Total
                248 kB/s |  14 MB     00:56     
    Running Transaction Check
    Running Transaction Test
    
    
    Transaction Check Error:
      installing package kernel-PAE-3.4.2-4.fc17.i686 needs 17MB on the /boot filesystem
      
      Error Summary
      -------------
      Disk Requirements:
        At least 17MB more space needed on the /boot filesystem.

恩，明显boo分区空间不足了（当初抠门的分了100M，不小吧，我x），现在只能删除旧的内核，先看有哪些吧:

    droid4u pts/2:python -> rpm -qa | grep kernel
    kernel-PAE-devel-3.3.4-5.fc17.i686
    kernel-PAE-3.4.0-1.fc17.i686
    kernel-PAE-3.3.4-5.fc17.i686
    kernel-PAE-3.3.7-1.fc17.i686
    kernel-headers-3.4.0-1.fc17.i686
    kernel-PAE-devel-3.4.0-1.fc17.i686
    kernel-PAE-devel-3.3.7-1.fc17.i686

查看当前自己使用的哪个？

    droid4u pts/2:python -> uname -r
    3.4.0-1.fc17.i686.PAE

ok，删除其他的即可：

    sudo yum remove xxx

然后继续更新就ok了：

    sudo yum update -y

ok，搞定！
