---
layout: post
title: GTest 1 -- Linux中配置gtest
category: Google Test
---

[下载](https://code.google.com/p/googletest/downloads/list)并解压后

1. ./configure

2. make

3. 编译完成后可以看到库文件在 **lib/.lib/** 目录下,编辑/etc/ld.so.conf文件，在新的一行添加上 **/your/path/to/** lib/.lib/,执行命令 sudo ldconfig

4. 配置项目的Makefile使用gtest,定义以下变量

{% highlight Makefile %}
GTEST_DIR = /your/path/to/gtest-1.7.0
GTEST_LIB = -pthread -L$(GTEST_DIR)/lib/.libs -lgtest -lgtest_main
GTEST_HEADER = -I$(GTEST_DIR)/include
GTEST = $(GTEST_HEADER) $(GTEST_LIB)
{% endhighlight %}

编译一个测试文件时(如test.cc)可以这么写：

{% highlight sh %}
g++ test.cc $(GTEST) -o test
{% endhighlight %}
