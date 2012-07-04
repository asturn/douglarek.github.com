---
layout: post
title: Python 装饰器
category : lang
tags : [python]
---
{% include JB/setup %}

接触Python还不是很长时间，昨天一个哥们问到装饰器的问题，说实话学习一门语言的时候，我没有一开始什么都搞懂的习惯，而且装饰器这个东西在Python中属于设计模式的一个东东，所以如果你对一门语言了解的深入以后这些东西理解起来就不那么费劲了，既然问我了，我也只能问谷歌同学了，下面只给出一个例子程序，相关的链接附在文后；

## 无参装饰器

    import time

     def timeit(func):
         def wrapper():
	         start = time.clock()
		 func()
	         end =time.clock()
		 print 'used:', end - start
         return wrapper

    @timeit
    def foo():
    print 'in foo()'

    foo()

## 带参装饰器

    def deco(func):
        def _deco(a, b):
            print("before myfunc() called.")
	    ret = func(a, b)
            print("  after myfunc() called. result: %s" % ret)
	    return ret
	return _deco

    @deco
    def myfunc(a, b):
        print(" myfunc(%s,%s) called." % (a, b))
        return a + b

    myfunc(1, 2)
    myfunc(3, 4)

这个例子虽小，但是这种编程方式属于面向切面编程（Aspect-Oriented Programming），多的不说，能暂时理解，会用而已：-）

关于装饰器的一些链接：

* [AstralWind](http://www.cnblogs.com/huxi/archive/2011/03/01/1967600.html)

* [cloudaice](http://www.cnblogs.com/cloudaice/archive/2012/01/27/python_func.html)

* [张云贵的博客](http://www.cnblogs.com/rhcad/archive/2011/12/21/2295507.html)
