---
layout: post
title: "解决 jenkins PermGen space 问题"
category: ci
tags: [jenkins, maven]
---
{% include JB/setup %}
要说Maven神奇，那是神奇的要死；7.23之前编译jenkins，无任何问题（当然这个也可能是jenkins的问题），完美的Build Sucess，但是7.23之后死活编译不成功了，出现了以下的编译错误：

    Exception in thread "main" java.lang.OutOfMemoryError: PermGen space
      at java.lang.ClassLoader.defineClass1(Native Method)
      at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631)
      at java.lang.ClassLoader.defineClass(ClassLoader.java:615)
      at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:141)
      at java.net.URLClassLoader.defineClass(URLClassLoader.java:283)
      at java.net.URLClassLoader.access$000(URLClassLoader.java:58)
      at java.net.URLClassLoader$1.run(URLClassLoader.java:197)
      at java.security.AccessController.doPrivileged(Native Method)
      at java.net.URLClassLoader.findClass(URLClassLoader.java:190)
      at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClassFromSelf(ClassRealm.java:386)
      at org.codehaus.plexus.classworlds.strategy.SelfFirstStrategy.loadClass(SelfFirstStrategy.java:42)
      at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:244)
      at org.codehaus.plexus.classworlds.realm.ClassRealm.loadClass(ClassRealm.java:230)
      at org.apache.maven.cli.MavenCli.execute(MavenCli.java:545)
      at org.apache.maven.cli.MavenCli.doMain(MavenCli.java:196)
      at org.apache.maven.cli.MavenCli.main(MavenCli.java:141)
      at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
      at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:39)
      at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:25)
      at java.lang.reflect.Method.invoke(Method.java:597)
      at org.codehaus.plexus.classworlds.launcher.Launcher.launchEnhanced(Launcher.java:290)
      at org.codehaus.plexus.classworlds.launcher.Launcher.launch(Launcher.java:230)
      at org.codehaus.plexus.classworlds.launcher.Launcher.mainWithExitCode(Launcher.java:409)
      at org.codehaus.plexus.classworlds.launcher.Launcher.main(Launcher.java:352)

什么意思呢？简单的讲就是OOM错误，内存溢出，无怪乎两种情况: 1. JVM使用的内存不足；2. 程序出现内存泄漏。

对于内存泄漏的排查是比较有技术的，现在的这个状况凭我的本事，只能试试了，假设是第一种错误，那就增加JVM运行内存：

    export MAVEN_OPTS='-Xmx512m -XX:MaxPermSize=128m'

通过上面的命令，那么使用Maven编译Jenkins的时候会使用我们定义的内存给JVM。

ok，搞定：-）
