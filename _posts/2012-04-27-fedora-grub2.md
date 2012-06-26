---
layout: post
title: Grub2 引导 fedora 16 硬盘安装 
category : linux
tags : [fedora, grub2]
---
{% include JB/setup %}

今天在公司，用着蛋疼的ubuntu 11.10，那慢的跟S一样，因为家里用的fedora 16，对gnome3慢慢喜欢的不得了，所以果断换了，当然第一天晚上下载DVD镜像，3.6G啊，所以不要等了，果断让它下着，第二天，满满的都在那。接下来，就开始使用grub2进行安装。

说实话，就现在linux版本的日新月异来说，刻盘安装简直就是浪费！既然机器上面已经安装了ubuntu，所以果断使用grub2引导了。

首先将下载的fedora iso文件放到某个分区（注意不能是ntfs分区，可以是fat32，ext3/ext4，否则无法识别，后果比较严重），这里拿我的做例子（/dev/sda4），怎么查看是sda多少呢？

    df
    文件系统           1K-块      已用      可用 已用% 挂载点
    rootfs         235151876   9307992 214072340    5% /
    devtmpfs         2039932         0   2039932    0% /dev
    tmpfs            2048604       256   2048348    1% /dev/shm
    /dev/sda5      235151876   9307992 214072340    5% /
    tmpfs            2048604     42352   2006252    3% /run
    tmpfs            2048604         0   2048604    0% /sys/fs/cgroup
    tmpfs            2048604         0   2048604    0% /media
    /dev/sda4      244319032 134652976  97253836   59% /work
    /dev/sda1         508745     72081    411064   15% /boot

或者使用fdisk -l（需要root）查看：

    fdisk -l
    Disk /dev/sda: 500.1 GB, 500107862016 bytes
    255 heads, 63 sectors/track, 60801 cylinders, total 976773168 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x320636aa
    
       Device Boot      Start         End      Blocks   Id  System
       /dev/sda1   *        2048     1026047      512000   83  Linux
       /dev/sda2         1026048     9414655     4194304   82  Linux swap / Solaris
       /dev/sda3         9414656   480279239   235432292    5  Extended
       /dev/sda4       480279240   976768064   248244412+  83  Linux
       /dev/sda5         9416704   480278527   235430912   83  Linux

ok,不多说，然后将iso中isolinux下的vmlinuz和initrd.img提取出来一同放入分区根目录下，然后重启进入grub2，接着敲击键盘的C键，进入grub2的命令行模式：

    grub >

下面才是重头戏，一系列的命令保证你进入fedora安装界面：

    grub > linux (hd0, msdos4)/vmlinuz linux repo=hd:/dev/sda4:/
    grub > initrd (hd0, msdos4)/initrd.img
    grub > boot

ok,进入系统，开始fedora安装之旅吧！
