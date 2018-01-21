---
title: 'C++中的##和#'
date: 2016-12-13 14:30:15
tags: C++
categories: 技术
---
>原文来自[http://www.2cto.com/kf/201503/381187.html](http://www.2cto.com/kf/201503/381187.html)

在C/C++的宏中，“#”的功能是将其后面的宏参数进行字符串化操作(Stringfication)，简单说就是在对它所引用的宏变量通过替换后在其左右各加上一个双引号。

而“##”被称为连接符(concatenator)，用来将两个子串Token连接为一个Token。注意这里连接的对象是Token就行，而不一定是宏的变量。还可以n个##符号连接n+1个Token，这个特性是“#”符号所不具备的。

凡是宏定义里有用“#”或“##”的地方宏参数是不会再展开。

若要使“#”和“##”的宏参数被展开，可以加多一层中间转换宏。加这层宏的用意是把所有宏的参数在这层里全部展开，那么在转换宏里的那一个宏就能得到正确的宏参数。

<!-- more -->

``` C++
#include "stdafx.h"
#include <iostream>
 
using namespace std;
 
//test1
#define WARN_IF(EXP) 
    if (EXP) 
    fprintf(stderr, warning: #EXP);
 
//test2
#define STR(s) #s
 
//test3
#define _STRI(s) #s
#define STRI(s) _STRI(s) //转换宏
 
//test4
#define paster(n) printf(token#n = %d, token##n)
 
//test5
#define _CONS(a, b) int(a##+##b)
#define CONS(a, b) _CONS(a, b) //转换宏
 
//test6
#define  _GET_FILE_NAME(f)   #f
#define  GET_FILE_NAME(f)    _GET_FILE_NAME(f)  //转换宏
 
//test7
#define  _TYPE_BUF_SIZE(type)  sizeof #type
#define  TYPE_BUF_SIZE(type)   _TYPE_BUF_SIZE(type) 
 
//test8
#define D(x)  #@x  //仅对单一标记转换有效
 
int main(int argc, char* argv[])
{
    //test1
    int divider = 0;
    WARN_IF(divider == 0);//warning: divider == 0 
 
    //test2
    printf(int max: %s, STR(INT_MAX));//int max: INT_MAX
 
    //test3
    printf(int max: %s, STRI(INT_MAX));//int max: 2147483647
 
    //test4
    int token9 = 9;
    paster(9);//token9 = 9 
 
    //test5
    int A = 15, B = 2;
    printf(A + B = %d, CONS(A, B));//A + B = 17
 
    //test6
    char  FILE_NAME[] = GET_FILE_NAME(__FILE__);
    cout<<file_name<<endl;
```