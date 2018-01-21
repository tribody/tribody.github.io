---
title: chrom浏览器font-size应用失效
date: 2017-02-25 16:45:13
tags: css
categories: 前端
---

what the hell!
今天学css调试的时候，应用`font-size: 10px`一直无效，半天无头绪，百度了才知道，chrome根本不支持`font-size<12px`的情况，开发者认为亚洲字符最小不应该小于12px！这是什么逻辑？

em和px两个单位的转换，在写css时，为了文本域相关标签设置的方便，可以在html下定义font-size，以此为基准，使用em单位来定义相关标签的padding，margin，line-height以及所有文字的font-size属性，会非常方便。

``` css
html {
	font-family: Arial, 'Microsoft YaHei';
	<!-- 以此为基准，该css应用的页面所有em单位都有1em = 15px -->
	font-size: 15px;
}
```