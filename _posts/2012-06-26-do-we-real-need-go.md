---
layout: post
title: Go语言：这个世界需要另外一门语言?
category : program
tags : [go]
---
{% include JB/setup %}

一直对于语言这个东西充满着好奇心，前辈常常会教导后辈：“c语言是精华语言，如果你精通了它，比如类似的语言Java，C++等，都是小kiss诸如此类的警世名言”，在我这行不通的，喜欢鼓捣，虽然不是很精通：

## 关于Go语言

Go是Google开发的一种编译型，并发型，并具有垃圾回收功能的编程语言。罗伯特·格瑞史莫（Robert Griesemer），罗勃·派克（Rob Pike）及肯·汤普逊于2007年9月开始设计Go语言，Go语言是基于Inferno操作系统所开发的。Go语言于2009年11月正式宣布推出，并在Linux及Mac OS X平台上进行了实现。Go 1（2012/03/28发布）是Go语言开发的一个主要里程碑，它为Go程序和项目提供了一个稳定的平台。

## Go语言的特点

Go编程语言是一个开源项目，其目的是提高开发人员的生产效率。Go语言的特点是表达力强、简明、整洁和高效。可以使用它的并发机制轻易地编写运行在多核或网络计算机上的程序，其新型的类型系统使程序的构建变得更加灵活和模块化。Go程序能快速地被编译为机器码，并且具有垃圾回收和运行时反射功能。它是一个快速的、静态类型的、编译型的语言，但使用起来却像一个动态类型的、解释型的语言。

---

祖师爷教导我们，学习一门语言的最简洁的途径就是用它，好，let‘s g:)

对于Go这个新生语言(它的来头一点也不小，Unix之父参与其中）来说，在社区的支持下，发展那是一日千里啊：P，所以获得它的源码，然后使用工具经常pull一下，Go托管于google code之上，使用Mercurial做版本控制，所以没有要首先安装一下：），然后克隆代码：

    hg clone -u release https://code.google.com/p/go

如果以后Go发布了新的版本，我们只需要更新它：

    cd go/src
    hg pull
    hg update release

ok，下面编译代码：P

    cd go/src
    ./all.bash

编译无误，完成之后，将bin路径放入环境变量

    export GOROOT=go_path
    export PATH=$PATH:$GOROOT/bin

如果上面一切妥当，我们不妨写个小的Go程序测试一下，一览Go的魅力：

    package main
    import "fmt"

    func main() {
        fmt.Printf("hello, world\n")
    }

然后，运行：

    go run hello.go

执行这一步很有意思，如果你以前使用过其他的脚本语言（譬如Python），你会发现上面的命令就是一个简单的解释过程，没有生成二进制文件，有解释语言的特性：），一方面，当我们执行一个build操作的时候，则生成二进制文件:

    go build hello.go

然后会在当前的目录下，生成这个对应文件的二进制文件（这里是hello），神奇吧，有编译语言的资质：）

上面的演示只是Go语言的冰山一角，我相信Go会带给我们更多的惊喜，我们只需要探索就OK，先走到这，有时间再完善：）

顺手写下几个Go语言启蒙网址：

[Go语言Wiki](ttp://golangwiki.org/wiki/index.php?title=%E9%A6%96%E9%A1%B5)

[Go官网（有时被墙)](http://golang.org)

[Go指南中文Week版（翻墙)](http://go-tour-zh.appspot.com/#1)

[Go语言中文官网](http://golang-china.org/)

[中文文档版本库(SVN)](http://code.google.com/p/golang-china/)
