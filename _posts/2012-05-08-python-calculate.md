---
layout: post
title: 用Python计算身份证校验码
category : lang
tags : [python]
---
{% include JB/setup %}

最近在疯狂的恶补Python（少壮不努力，长大编程序），偶然发现一个仁兄的文章，大喜过望，转过来了，再一次印证了Python的强大、便利，言归正传，转贴地址： [用Python计算身份证校验码](http://www.keakon.net/2009/03/07/%E7%94%A8Python%E8%AE%A1%E7%AE%97%E8%BA%AB%E4%BB%BD%E8%AF%81%E6%A0%A1%E9%AA%8C%E7%A0%81)。

原来的天朝良民证是15位，构成如下：
* 1～6位：地址码。采用的是行政区划代码，可以去统计局的网站查。
* 7～12位：生日期码。构成为yymmdd。
* 13～15位：顺序码。每个地区出生人口按顺序递增，最后一位奇数分给男的，偶数分给女的。

18位则有2点改动：
* 生日期码变为8位，构成为yyyymmdd。
* 增加校验码，即第18位。按照ISO 7064:1983.MOD 11-2校验码计算。

计算方法很无聊：

    将身份证号码的前17位数分别乘以不同的系数。从第一位到第十七位的系数分别为：7 9 10 5 8 4 2 1 6 3 7 9 10 5 8 4 2
    将这17位数字和系数相乘的结果相加。
    用加出来和除以11，得到余数。
    余数的结果只可能为0 1 2 3 4 5 6 7 8 9 10这11种，分别对应的最后一位身份证的号码为1 0 X 9 8 7 6 5 4 3 2。

弄懂这个后，很快就能写出Python的计算程序了：
{% highlight python %}
    s = "34052419800101001" #这个是要查的身份证号码的前17位
    #计算总和
    sum = int(s[0]) * 7 + int(s[1]) * 9 + int(s[2]) * 10 + int(s[3]) * 5 + int(s[4]) * 8 + int(s[5]) * 4 + int(s[6]) * 2 + int(s[7]) * 1 + int(s[8]) * 6 + int(s[9]) * 3 + int(s[10]) * 7 + int(s[11]) * 9 + int(s[12]) * 10 + int(s[13]) * 5 + int(s[14]) * 8 + int(s[15]) * 4 + int(s[16]) * 2
	      
    #输出校验码
    print '10X98765432'[sum % 11]
{% endhighlight %}
有没有觉得计算总和非常无语，下面来简化代码：
{% highlight python %}
    s = "34052419800101001"
         
    #分组
    temp = zip(s[0:17], [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2])
    print temp
		          
    #相乘
    temp2 = map(lambda x:int(x[0])*x[1], temp)
    print temp2
	           
    #相加
    temp3 = sum(temp2)
    #或者这样写：
    #temp3 = reduce(lambda x, y : x + y, temp2)
    print temp3
							            
    #最终结果
    print '10X98765432'[temp3 % 11]
					         
    #写成一行代码就是这样
    print '10X98765432'[sum(map(lambda x: int(x[0]) * x[1], zip(s[0:17], [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]) )) % 11]
    #print '10X98765432'[reduce(lambda x, y: x + y, map(lambda x: int(x[0]) * x[1], zip(s[0:17], [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2]) )) % 11]
{% endhighlight %}
可能不熟悉Python的还看不懂怎么zip、map和reduce的作用，我再解释下吧。（sum太简单了，就不说了，相当于reduce的简化版。）

zip是迭代各个参数，并返回一个元组的列表。第i个元组由参数的第i个元素组成。当一个参数迭代完成后，就结束zip，其余参数未迭代的部分忽略。
举例来说：
{% highlight python %}
    >>> a = [1, 2, 3]
    >>> b = [4, 5, 6]
    >>> zip(a, b)
    [(1, 4), (2, 5), (3, 6)]
    >>> zip([1, 2], *[(3, 4), (5, 6)]) #星号(*)是把列表的元素转换为参数
    [(1, 3, 5), (2, 4, 6)]
    >>> zip(*zip(a, b)) #相当于unzip
    [(1, 2, 3), (4, 5, 6)]
    >>> (x, y) = zip(*zip(a, b))
    >>> x
    (1, 2, 3)
    >>> y
    (4, 5, 6)
    >>> c = [7, 8, 9, 10]
    >>> zip(a, c)
    [(1, 7), (2, 8), (3, 9)]
    >>> zip(a, b, c)
    [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
    >>> d = 'abcd'
    >>> zip(c, d)
    [(7, 'a'), (8, 'b'), (9, 'c'), (10, 'd')]
{% endhighlight %}
map则是将一个函数迭代处理各个参数，返回结果列表。与zip不同的是，如果有个参数比较短，迭代完它后将用None来代替不足的元素，如果None不支持该操作，可能会抛出异常。演示：
{% highlight python %}
    >>> map(lambda x: 2 * x, [1, 2, 3])
    [2, 4, 6]
    >>> map(lambda x: x[0] + x[1], [(1, 4), (2, 5), (3, 6)])
    [5, 7, 9]
    >>> map(lambda x, y: x + y, [1, 2, 3], [4, 5, 6])
    [5, 7, 9]
    >>> map(lambda x, y: x + y, [1, 2, 3], [4, 5, 6, 7])
    Traceback (most recent call last):
    File "", line 1, in
    File "", line 1, in
    TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
    #最后的None + 7会出错
{% endhighlight %}
reduce是用一个函数从左至右依次迭代处理各个元素，并返回最后的总结果。此外，如果有第3个参数的话，会将第3个参数当成初始值。
{% highlight python %}
    >>> reduce(lambda x, y: x + y, [1, 2, 3, 4, 5]) #计算((((1+2)+3)+4)+5)
        15
    >>> reduce(lambda x, y: x + y, range(101)) #从1加到100
        5050
    >>> reduce(lambda x, y: x * y, range(1, 11)) #计算10的阶乘
        3628800
    >>> print reduce(lambda x, y: str(x) + str(y), range(11), '输出1~10: ')
        输出1~10: 012345678910
    >>> print reduce(lambda x, y: (x + '%d') % y, range(11), '输出0~10: ')
        输出0~10: 012345678910
    >>> print (lambda n, m: reduce(lambda x, y: x + n ** y, xrange(m + 1)))(3, 4) #计算n+n^2+n^3....n^m，n和m我给了4
        120
    >>> (lambda n: reduce(lambda x, y: x * y, xrange(1, n + 1)))(10) # 计算10的阶乘（虽然我没优化算法，但计算10000的阶乘也不用1秒）
        3628800
{% endhighlight %}
Python果然是非常方便的东西啊～
