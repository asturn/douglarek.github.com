---
layout: post
title: Fedora 16 下编译 android 4.0.4
category : develop
tags : [fedora, android]
---
{% include JB/setup %}

最近也是从ubuntu下迁移到fedora中来，以前也用过fedora，但是效果不是很好，自从gnome3出来以后，觉得unity经ubuntu开发后感觉很一般，为了视觉效果，果断玩起fedora，gnome3真的很帅，尽管和其他一些新鲜事物一样，刚出现总是受到各方面的质疑，说白了还是个人习惯的问题，现在已经基本适应gnome3，fedora 16使用起来也很给力。

从ubuntu下迁移过来，首要的一个问题需要解决，那就是如何解决android编译问题？我们知道android源码的编译，[android官方](http://source.android.com/)推荐64位ubuntu（10.04--11.10)，对于其他linux版本的编译，完全需要自己的琢磨，不得不说android，你大爷的。说归说，骂归骂，还是要靠android吃饭的，所以自己来搞定一切吧（这也是linux geek的一个习惯）：P

闲言少叙，且归正题。既然编译android，那么一些必备的工具是必须的，在这方面，ubuntu当然有官方的详细资料，对于fedora的建议：装系统时，请将编译链（gcc，g++，相关编译库）全部安装，否则出现了问题则得不尝失。
一般需要的工具： 
> Python 2.5--2.7

> Gnu make 3.8.1

> JDK 1.6（非OpenJdk1.6）

> Git 1.7+
		
下面就一步一步编译，一步一步的解决问题：

    cd src_root
    . build/envsetup.sh
    lunch full_eng
    make

首先出现的第一个问题是make的版本问题，fedora 16默认make版本是3.8.2，如果执行make后会出现下面的类似错误：

    warning *  Android can only be built by version 3.81.)

解决的方法，很简单，编辑build/core/main.mk：

    vim build/core/main.mk

注释掉41--48行就ok。
接下来你会遇到g++版本兼容性问题，例如发生在frameworks/compile/slang/slang_rs_export_foreach.cpp文件的编译错误：

    frameworks/compile/slang/slang_rs_export_foreach.cpp: 在静态成员函数
    ‘static slang::RSExportForEach* slang::RSExportForEach::Create(slang::RSContext*, const clang::FunctionDecl*)’中:
    frameworks/compile/slang/slang_rs_export_foreach.cpp:249:23: 错误：variable ‘ParamName’ set but not used [-Werror=unused-but-set-variable]
    cc1plus: all warnings being treated as errors
    make: *** [out/host/linux-x86/obj/EXECUTABLES/llvm-rs-cc_intermediates/slang_rs_export_foreach.o] 错误 1

解决方法，注释掉249行代码：

    llvm::StringRef ParamName = PVD->getName();

接下来是一个perl的编译问题，缺少gperf，则安装；然后是fedora 16中的perl版本将switch的module拿掉了，出现下面的编译错误：

    Can't locate Switch.pm in @INC (@INC contains: /usr/local/lib/perl5 
    /usr/local/share/perl5 /usr/lib/perl5/vendor_perl /usr/share/perl5/vendor_perl /usr/lib/perl5 /usr/share/perl5 .) 
    at external/webkit/Source/WebCore/make-hash-tools.pl line 23.
    BEGIN failed--compilation aborted at external/webkit/Source/WebCore/make-hash-tools.pl line 23.

解决方法是修改external/webkit/Source/WebCore/make-hash-tools.pl文件（以下解决方法均采用git diff方式显示）：

    diff --git a/Source/WebCore/make-hash-tools.pl b/Source/WebCore/make-hash-tools.pl
    index 37639eb..9f85002 100644
    --- a/Source/WebCore/make-hash-tools.pl
    +++ b/Source/WebCore/make-hash-tools.pl
    @@ -20,7 +20,7 @@
     #   Boston, MA 02110-1301, USA.
      
       use strict;
      -use Switch;
      +use feature qw(switch);
       use File::Basename;
 
       my $outdir = $ARGV[0];
     @@ -28,9 +28,9 @@ shift;
       my $option = basename($ARGV[0],".gperf");
 	    
 	     
        -switch ($option) {
        +given ($option) {
	      
        -case "DocTypeStrings" {
        +when ("DocTypeStrings") {
	       
        my $docTypeStringsGenerated    = "$outdir/DocTypeStrings.cpp";
        my $docTypeStringsGperf        = $ARGV[0];
      @@ -40,7 +40,7 @@ case "DocTypeStrings" {
			  
        } # case "DocTypeStrings"
			    
        -case "ColorData" {
          +when ("ColorData") {
			     
         my $colorDataGenerated         = "$outdir/ColorData.cpp";
         my $colorDataGperf             = $ARGV[0];

接下来还有很多的错误，不再一一贴出具体错误，集中一下：

    diff --git a/core/combo/HOST_linux-x86.mk b/core/combo/HOST_linux-x86.mk
    index 5ae4972..7df2893 100644
    --- a/core/combo/HOST_linux-x86.mk
    +++ b/core/combo/HOST_linux-x86.mk
    @@ -53,6 +53,6 @@ HOST_GLOBAL_CFLAGS += \
            -include $(call select-android-config-h,linux-x86)
	     
	      # Disable new longjmp in glibc 2.11 and later. See bug 2967937.
	      -HOST_GLOBAL_CFLAGS += -D_FORTIFY_SOURCE=0
	      +HOST_GLOBAL_CFLAGS += -U_FORTIFY_SOURCE -D_FORTIFY_SOURCE=0
	       
	        HOST_NO_UNDEFINED_LDFLAGS := -Wl,--no-undefined

development/tools/emulator/opengl/host/renderer/Android.mk文件:

    diff --git a/tools/emulator/opengl/host/renderer/Android.mk b/tools/emulator/opengl/host/renderer/Android.mk
    index 55fcb80..613179c 100644
    --- a/tools/emulator/opengl/host/renderer/Android.mk
    +++ b/tools/emulator/opengl/host/renderer/Android.mk
    @@ -6,6 +6,8 @@ $(call emugl-import,libOpenglRender)
     LOCAL_SRC_FILES := main.cpp
      LOCAL_CFLAGS    += -O0 -g
       
       +LOCAL_LDLIBS += -lX11
       +
        #ifeq ($(HOST_OS),windows)
	 #LOCAL_LDLIBS += -lws2_32
	  #endif

external/gtest下修改的文件:

    diff --git a/include/gtest/internal/gtest-param-util.h b/include/gtest/internal/gtest-param-util.h
    index 5559ab4..2927f02 100644
    --- a/include/gtest/internal/gtest-param-util.h
    +++ b/include/gtest/internal/gtest-param-util.h
    @@ -37,6 +37,8 @@
     #include <iterator>
      #include <utility>
       #include <vector>
       +#include <stddef.h>
       +#include <cstddef>
        
	 #include <gtest/internal/gtest-port.h>
	  
	  diff --git a/src/Android.mk b/src/Android.mk
	  index 2465e16..6e3facb 100644
	  --- a/src/Android.mk
	  +++ b/src/Android.mk
	  @@ -49,7 +49,7 @@ LOCAL_SRC_FILES := gtest-all.cc
	   
	    LOCAL_C_INCLUDES := $(libgtest_host_includes)
	     
	     -LOCAL_CFLAGS += -O0
	     +LOCAL_CFLAGS += -O0 -Wno-missing-field-initializers
	      
	       LOCAL_MODULE := libgtest_host
	        LOCAL_MODULE_TAGS := eng

external/llvm/llvm-host-build.mk文件:

    diff --git a/llvm-host-build.mk b/llvm-host-build.mk
    index 5219efd..53a6229 100644
    --- a/llvm-host-build.mk
    +++ b/llvm-host-build.mk
    @@ -10,6 +10,8 @@ LOCAL_CFLAGS :=       \
            -Wwrite-strings \
	            $(LOCAL_CFLAGS)
		     
		     +LOCAL_LDLIBS := -lpthread -ldl
		     +
		      ifeq ($(LLVM_ENABLE_ASSERTION),true)
		       LOCAL_CFLAGS :=        \
		               -D_DEBUG        \

external/mesa3d/src/glsl/linker.cpp文件:

    diff --git a/src/glsl/linker.cpp b/src/glsl/linker.cpp
    index f8b6962..f31e5b5 100644
    --- a/src/glsl/linker.cpp
    +++ b/src/glsl/linker.cpp
    @@ -67,6 +67,7 @@
     #include <cstdio>
      #include <cstdarg>
       #include <climits>
       +#include <stddef.h>
        
	 #include <pixelflinger2/pixelflinger2_interface.h>
	  
	  @@ -1762,4 +1763,4 @@ done:
	      }
	       
	           //hieralloc_free(mem_ctx);
		   -}
		   \ No newline at end of file
		   +}

external/oprofile/libpp/format_output.h文件:

    diff --git a/libpp/format_output.h b/libpp/format_output.h
    index b6c4592..8e527d5 100644
    --- a/libpp/format_output.h
    +++ b/libpp/format_output.h
    @@ -91,7 +91,7 @@ protected:
                    symbol_entry const & symbol;
		                    sample_entry const & sample;
				                    size_t pclass;
						    -               mutable counts_t & counts;
						    +               counts_t & counts;
						                    extra_images const & extra;
								                    double diff;
										            };

这还不算完，接下来是一个java编译错误：

    target Java: CubeLiveWallpapers (out/target/common/obj/APPS/CubeLiveWallpapers_intermediates/classes)
    PassFailButtons.java:191: android.app.Activity 中的 onCreateDialog(int,android.os.Bundle) 
    无法实现 com.android.cts.verifier.PassFailButtons.PassFailActivity 中的 onCreateDialog(int,android.os.Bundle)；
    正在尝试指定更低的访问权限；为 public
        private static 
	                    ^
			    注意：某些输入文件使用或覆盖了已过时的 API。
			    注意：要了解详细信息，请使用 -Xlint:deprecation 重新编译。
			    1 错误
			    make: *** [out/target/common/obj/APPS/CtsVerifier_intermediates/classes-full-debug.jar] 错误 41

解决方法如下：

    diff --git a/apps/CtsVerifier/src/com/android/cts/verifier/PassFailButtons.java
    b/apps/CtsVerifier/src/com/android/cts/verifier/PassFailButtons.java
    index 9991b9d..f091c75 100644
    --- a/apps/CtsVerifier/src/com/android/cts/verifier/PassFailButtons.java
    +++ b/apps/CtsVerifier/src/com/android/cts/verifier/PassFailButtons.java
    @@ -77,7 +77,7 @@ public class PassFailButtons {
             Button getPassButton();
	      
	               /* Added to the interface just to make sure it isn't forgotten in the implementations. */
		       -        Dialog onCreateDialog(int id, Bundle args);
		       +        // Dialog onCreateDialog(int id, Bundle args);
		        
			         /**
				           * Returns a unique identifier for the test.  Usually, this is just the class name.
参考网址：
* [http://www.linuxidc.com/Linux/2011-12/49257.htm](http://www.linuxidc.com/Linux/2011-12/49257.htm)
* [http://www.qhm123.com/2012/02/6/fedora-16-x64-compile-android-4.0.3-source-code.html](http://www.qhm123.com/2012/02/6/fedora-16-x64-compile-android-4.0.3-source-code.html)
