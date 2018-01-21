---
title: 算法学习之union-find
date: 2016-12-16 18:13:18
tags: 算法
catogries: 技术
---

最近开始在跟进coursera的算法课程，为明年的实习面试做准备。Sedgewick的这门算法课程非常出名，配套的有一本书叫做《Algorithms》,和Coursera上的课程配套。

第一周课程先以一个案例即union-find算法为例，阐述算法分析的基础。

##动态连通性
union-find算法要解决的是一个连通性问题。首先，问题描述：
给定一个有N个对象的集合，定义如下操作:
- Union command: 连接两个对象。
- Find/connected query：两个对象之间是否是连通的。

<!-- more -->

该问题有许多应用场景，如迷宫寻路，物理化学中的渗流和通信网络中的连通性等等。

算法的API定义：
``` Java
public class UF
	UF(int N)							inial union-find data stucture with N objects(0 to N-1)
	void union(int p, int q)			add connection between p and q
	boolean connected(int p, int q)		are p and q in the same component?
	int find(int p)						component identifier for p(0 to N-1)
	int count() 						number of component
```

动态连通性client
``` Java
publci static void main(String[] args) {
	int N = StdIn.readInt();
	UF uf = new UF(N);
	while (!StdIn.isEmpty()) {
		int p = StdIn.readInt();
		int q = StdIn.readInt();
		if (!uf.connected(p, q)) {
			uf.union(p, q);
			StdOut.println(p + " " + q);
		}
	}
}
```

输出实例：

![动态连通性](/img/client.png)

课程实现了两种UnionFind算法，逐渐优化。

## Quick-find
第一种使用暴力方法，每次union(p, q)，将所有值为q的索引的值更新为q

``` Java
public class QuickFindUF {
	private int[] id;

	public QuickFindUF(int N) {
		id = new int[N];
		for(int i = 0; i < N; i++)
		id[i] = i;
	}

	public boolean connected(int p, int q) {
		return id[p] = id[q];
	}

	public void union(int p, int q) {
		int pid = id[p];
		int qid = id[q];
		for (int i = 0; i < id.length; i++) 
			if (id[i] == pid) id[i] = qid;
	}
}
```

## Quick-uinion

使用数据结构“树”，同一子集有相同的祖先节点（根节点）。

``` Java
public class QuickUnionUF {
	private int[] id;

	public QuickUnionUF(int N) {
		id = new int[N];
		for (int i = 0; i < N; i++)
		id[i] = i;
	}

	private int root(int i) {
		while (i != id[i]) i = id[i];
		return i;
	}

	public boolean connected(int p, int q) {
		retur root(p) == root(q);
	}

	public void union(int p, int q) {
		int i = root(p);
		int j = root(q);
		id[i] = j;
	}
}
```

### 改进一：加权

防止“树枝”过长，根据权重调节树枝长度。

修改`union`：
``` Java
int i = root(p);
int j = root(q);
if (i == j) return;
if (sz[i] < sz[j]) {
	id[i] = j;
	sz[j] += sz[i];
} else {
	id[j] = i;
	sz[i] += sz[j];
}
```

### 改进二：路径压缩

``` Java
private int root(int i) {
	while (i != id[i]) {
		id[i] = id[id[i]];
		i = id[i];
	}
	return i;
}
```

## 课程作业——渗透问题(Pecolation)

问题描述的网址在[Programming Assignment 1: Pecolation](http://coursera.cs.princeton.edu/algs4/assignments/percolation.html)

**寒假有空整理下，未完待续......**