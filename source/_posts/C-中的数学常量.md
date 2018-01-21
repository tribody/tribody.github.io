---
title: C++中的数学常量
date: 2017-12-26 18:15:05
tags: C++
categories: 技术
---

在编译一个C文件的时候，编译器报错找不到常量`M_LN10`，google了一下，发现和编译器版本有关系。早期的C和C++标准库和编译器会在它们的`math.h`和`cmath`头文件中提供许多的数学常量，但是Visual Studio自2015起彻底移除了包含这些常量的头文件，因此会出现找不到标识符的编译错误。Visual Studio2012仍然保留这些头文件，但是需要声明`_USE_MATH_DEFINES`才能够使用。不太明白微软为什么要这么做？是为了给用户更多自定义的权利吗？这样代码的兼容性不是存在很大的问题？

因此，遇到此类问题，只能通过手动定义相关常量解决。比较友好的是，GCC仍然保留这些常量，因此使用GCC的Linux和Mac OSX用户不用担心这个问题了。

>参考自[https://codeyarns.com/2010/04/13/math-constants-for-cpp/](https://codeyarns.com/2010/04/13/math-constants-for-cpp/)