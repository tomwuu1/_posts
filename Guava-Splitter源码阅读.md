---
title: Guava-Splitter源码阅读
date: 2024-07-22 14:19:59
tags:
---

# 属性

`CharMatcher trimmer`：可以在分割字符串之后，去除结果字符串的前后字符。

`boolean omitEmptyStrings`：是否要丢掉空的结果

`int limit`：返回个数限制

`Strategy strategy`：策略模式的入口

# 内部类/接口

## Strategy

策略模式的入口

## SplittingIterator

实现了字符串拆分的逻辑

# 方法

## split

返回一个迭代器，在进行调用的时候再进行分割，采用了懒加载的思路。
