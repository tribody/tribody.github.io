---
title: for iterator
categories: 技术
date: 2016-12-09 08:45:58
tags: C++
---

昨天看了下C++的并行运算实现方式，编程实现有两种方式，MPI和OpenMP。知乎上有做比较，暂作摘录，区别在于：
- OpenMP：线程级（并行粒度）；共享存储；隐式（数据分配方式）；可扩展性差；
- MPI：进程级；分布式存储；显式；可扩展性好。

OpenMP采用共享存储，意味着它只适应于SMP,DSM机器，不适合于集群。MPI虽适合于各种机器，但它的编程模型复杂：
- 需要分析及划分应用程序问题，并将问题映射到分布式进程集合；
- 需要解决通信延迟大和负载不平衡两个主要问题；
- 调试MPI程序麻烦；
- MPI程序可靠性差，一个进程出问题，整个程序将错误；
<!-- more -->

>作者：李超铮
链接：[https://www.zhihu.com/question/20188244/answer/14552204](https://www.zhihu.com/question/20188244/answer/14552204)
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

最近在移植一份linux c++代码，正好用到了OpenMP。使用很简单，直接VS开启OpenMP支持，然后在代码前添加#pragma注释即可。

当然前面都是背景...

并行计算最适合的场景是for循环，然后就顺带了解了下c++11支持的for循环结构。众所周知，java在1.5JDK版本就开始支持了增强for循环，有多好用不用我多说了，现在基本语言都支持了增强for循环，C++11也实现了。以下为C++11支持的5种for循环方式：
``` c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	vector<int> numSet;

	for (size_t i = 0; i < numSet.size(); i++);

	for (auto itr = numSet.begin(); itr != numSet.end(); itr++);

	for (auto item : numSet);

	for each(auto item in numSet);

	std::for_each(numSet.begin(), numSet.end(), [&](int i){;});

	return 0;
}
```
按轮子哥的说法，
第一个不好，万一numSet[i]不是O(1)的呢，容器可不止vector一种。
第二个太长了。
**第三个是推荐的。**
第四个是VC++的C++/CLI或C++/CX扩展，不过他跟第三个其实完全没有区别，因为互相都可以用。
第五个很难讲，主要是你要把整个程序写成函数式或响应式的，用这个才有额外的好处。

>作者：vczh
链接：[https://www.zhihu.com/question/28127777/answer/39500842](https://www.zhihu.com/question/28127777/answer/39500842)
来源：知乎
著作权归作者所有，转载请联系作者获得授权。

