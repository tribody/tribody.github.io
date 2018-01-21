---
title: C++学习笔记
date: 2016-12-13 14:57:51
tags: C++
categories: 技术
---

## C++中的宏
由于很长时间没有摸C++这门儿语言了，把宏定义简单理解成了类似于变量声明和函数声明式地东西。然而，爬了一下博客才发现，宏仅仅只会在在预处理过程中展开，做简单的字符串替换，而`没有任何的计算过程`。

两篇关于C++宏的相关讲解。
- [http://www.cnblogs.com/sopic/archive/2013/12/27/3493877.html](http://www.cnblogs.com/sopic/archive/2013/12/27/3493877.html),这一篇强烈推荐。
- [http://blog.chinaunix.net/uid-21372424-id-119797.html](http://blog.chinaunix.net/uid-21372424-id-119797.html)

<!-- more -->

## C++中的Lambda函数（匿名函数）
更新，今天学习了一下C++11标准中的lambda函数（匿名函数）。
Lambda函数的总体形式是：
```
[captures] (params) -> ret {Statements;}
```
`captrues`表示变量截取，
- []不截取任何变量
- [&]截取外部作用域中所有变量，并作为引用在函数体中使用
- [=]截取外部作用域中所有变量，并拷贝一份在函数体中使用
- [-, &foo]截取外部作用域中所有变量，并拷贝一份在函数体中使用，但对`foo`变量使用引用
- [bar]截取`bar`变量并且拷贝一份在函数体中使用，同时不截取其他变量
- [this]截取当前类中的`this`指针。如果已经使用了`&`或者`=`就默认添加此选项

`params`表示参数列表，
`ret`表示返回值类型，
`statements`就是函数体了。

>参考自[http://www.cnblogs.com/lidabo/p/3908663.html](http://www.cnblogs.com/lidabo/p/3908663.html)

## C++中构造函数之间的调用
偶然看一份代码的时候，发现C++构造函数之间时可以互相调用的，这样可以减少重载构造函数时所需的代码量。这种构造函数之间的调用称为`委托`或`转接`（delegation)，Java以及C#都支持这种功能。例如：
``` C++
class SomeType {
  int number;
  string name;
  SomeType( int i, string& s ) : number(i), name(s){}
public:
  SomeType( )           : SomeType( 0, "invalid" ){}
  SomeType( int i )     : SomeType( i, "guest" ){}
  SomeType( string& s ) : SomeType( 1, s ){ PostInit(); }
};
```
>相关内容参考自[http://www.cnblogs.com/ayanmw/archive/2012/08/20/2647808.html](http://www.cnblogs.com/ayanmw/archive/2012/08/20/2647808.html)