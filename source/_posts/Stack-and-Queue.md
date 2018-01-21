---
title: Stacks and Queues
date: 2016-12-27 20:39:22
tags: 算法
categories: 技术
---

堆栈和队列都可以通过链表和数组分别来实现。

在堆栈和队列的实现过程中，使用到了泛型和接口的技术。

## 堆栈

堆栈是一种先进后出的存储结构(FILO)。

在用数组实现堆栈的时候需要在一定情况下调整数组大小。

<!-- more -->

``` Java
public class ArrayStack<Item> {

	private Item[] s;
	private int N = 0;
	
	public ArrayStack() {
		s = new Item[1];
	}

	public void push(Item item) {
		if (N == s.length) resize(2 * s.length);
		s[N++] = item;
	}

	// 避免反复调整数组大小，选择1/4
	public Item pop() {
		if (N == 1/4 * s.length) resize(s.length/2);
		Item item = s[--N];
		s[N] = null;
		return item;
	}

	public boolean isEmpty() {
		return N == 0;
	}

	public int size() {
		return N;
	}

	private resize(int capacity) {
		Item[] copy = new Item[capacity];
		for (int i = 0; i < N; i++)
			copy[i] = s[i];
		s = copy;
	}
}
```

链表的实现方式比较简单。

``` Java
public class LinkedStack<Item> {

	private Node first = null;
	private int N = 0;

	private class Node {
		Item item;
		Node next;
	}

	public void push(Item item) {
		Node oldfirst = first;
		first = new Node();
		first.item = item;
		first.next = oldfirst;
		N++;
	}

	public Item pop() {
		Item item = first.item;
		first = first.next;
		return item;
		N--;
	}

	public boolean isEmpty() {
		return first == null;
	}

	public int size() {
		return N;
	}
}
```

## 队列

队列是一种先进先出的存储结构(FIFO)。

数组实现

``` Java
public class ArrayQueue<Item> {
	
	private Item[] q;
	private int tail = head = N = 0;

	public ArrayQueue() {
		q = new Item[1];
	}

	public enqueue(Item item) {
		if (N == q.length) resize(2 * q.length);
		if (tail == q.length) tail = 0;
		q[tail++] = item;
		N++; 
	}

	public Item dequeue() {
		Item item = q[head];
		q[head++] = null;
		N--;
		if (head == q.length) head = 0;
		if (N == s.length/4) resize(s.lenght/2);
		return item;
	}

	private resize(int capacity) {
		Item copy = new Item[capacity];
		for (int i = 0; i < N; i++)
			copy[i] = q[(i + head)%s.length];
		s = copy;
		head = 0;
		tail = N;
	}

	public int size() {
		return N;
	}

	public boolean isEmpty() {
		return N == 0;
	}
}
```

链表实现方式比较简单。

``` Java
public class LinkedQueue<Item> {
	private Node first, last;
	private int N;

	private class Node {
		Item item;
		Node next;
	}

	public void enqueue(Item item) {
		Node oldlast = last;
		last = new Node();
		last.item = item;
		last.next = null;
		if (isEmpty()) first = last;
		else oldlast.next = last;
		N++;
	}

	public Item dequeue() {
		Item item = first.item;
		first = first.next;
		if (isEmpty()) last = null;
		N--;
		return item;
	}

	public int size() {
		return N;
	}

	public boolean isEmpty() {
		return N == 0;
	}
}

```

## 课程作业——双端队列和随机队列

## shuffle算法(Fisher-Yates shuffle and Knuth shuffle)

## 水塘抽样算法(Reservoir sampling)

## 算法实现

**未完待续**
