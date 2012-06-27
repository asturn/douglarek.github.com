---
layout: post
title: Python解惑：main方法？
category : lang
tags : [python]
---
{% include JB/setup %}

最近由于工作的需要，开始了Python的修炼，一门语言有一门语言的规则，Python也不例外；自己修炼了那么久，忽然发现了一个现实的问题，Python的main方法怎么玩的？为什么一个py程序，没有main方法，也可以运行？

下面来自于前辈的一些说法，很是解惑，下面就摘取一下：

* [http://blog.csdn.net/noodies/article/details/6034105](http://blog.csdn.net/noodies/article/details/6034105)

    在C/C++/Java中，main是程序执行的起点，Python中，也有类似的运行机制，但方式却截然不同：Python使用缩进对齐组织代码的执行，所有没有缩进的代码（非函数定义和类定义），都会在载入时自动执行，这些代码，可以认为是Python的main函数。

    每个文件（模块）都可以任意写一些没有缩进的代码，并且在载入时自动执行，为了区分主执行文件还是被调用的文件，Python引入了一个变量__name__，当文件是被调用时，__name__的值为模块名，当文件被执行时，__name__为'__main__'。这个特性，为测试驱动开发提供了极好的支持，我们可以在每个模块中写上测试代码，这些测试代码仅当模块被Python直接执行时才会运行，代码和测试完美的结合在一起。

* [http://blog.csdn.net/huangxiansheng1980/article/details/7196951](http://blog.csdn.net/huangxiansheng1980/article/details/7196951)

    python并不像c/c++那样由main函数作为入点函数，而是每个以py为后缀的文件可以单独执行，并且从文件中的第一行执行到最后一行。那么你可能要问一个问题，那么我们一个python工程中可能不止一个py文件，而是很多个，那么每个都能执行那不是乱套了，怎么办呢？

    有办法，请看下面。

    每个py文件被解释器解释执行的时候，都会默认有一个__name__内置的变量，如果这个py文件是被解释器直接解释执行，而不是被其他的py文件通过import后，调用执行，那么这个__name__变量就是__main__，否则就是该文件的文件名去掉.py后的名字。

    既然这样，那我们就可以在文件的一开始，加上这么一句：

        if __name__ != '__main__'  
                quit()

    这样只有我们的主py文件才会最开始执行。

    不过，实际上，这句是用不上的，因为被别人调用py文件中一般都是函数和类的定义，没有实际执行的东西，所以你要是执行它，也没有任何结果。
