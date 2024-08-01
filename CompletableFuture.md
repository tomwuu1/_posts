---
title: CompletableFuture
date: 2024-07-18 15:06:34
tags:
---

# 创建

supplyAsync/runAsync的区别是一个不能获取结果

```java
ExecutorService executorService = Executors.newFixedThreadPool(5);
CompletableFuture<String> completableFuture1 = CompletableFuture.supplyAsync(() -> {
    System.out.println("1");
    return "1";
}, executorService);
```

# 组合

thenApply/thenCombine，allOf/anyOf

```java
CompletableFuture<String> completableFuture2 = completableFuture1.thenApply(result1 -> {
    System.out.println("2");
    return "2";
});
CompletableFuture<String> completableFuture3 = CompletableFuture.supplyAsync(() -> {
    System.out.println("3");
    return "3";
}, executorService);
CompletableFuture<String> completableFuture4 = completableFuture2.thenCombine(completableFuture3, (result2, result3) -> {
   System.out.println("4");
   return "4";
});
```

join/get 获取结果的区别在与get需要显式地处理异常

# 原理

基于观察者模式

两个字段：result，stack（Completion）

stack是在获取结果之后要通知的completablefuture的栈

![图13 执行流程简要说明](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/f449bbc62d4a1f8e9e4998929196513d165269.gif)

## 一元依赖

1. 在观察者入栈前，判断当前CompletableFuture是否完成用，没完成入栈，已完成直接触发
2. 入栈
3. 再次检查
4. 完成之后通知

- 如何避免同一个CompletableFuture被同时回调？通过设置一个状态位，在执行之前通过CAS设置

## 二元依赖

和一元依赖触发过程主要的区别在于，它在一个CompletableFuture完成后，都会检查两个依赖是否都完成。

## 多元依赖

`allOf`，`anyOf`两者的实现方式是将被依赖的CompletableFuture构建成平衡二叉树



# 线程执行问题

同步方法（无Async后缀）：

- 注册时被依赖的操作已经执行完，由当前线程执行。
- 还未执行完，由回调线程执行。

异步方法：

- 传了Executor，由指定线程池执行
- 当不传递Executor时，会使用ForkJoinPool中的共用线程池CommonPool？？？

异常处理：

由于异常信息存在线程栈当中，要用异常捕获回调exceptionally。

# 参考文章

https://tech.meituan.com/2022/05/12/principles-and-practices-of-completablefuture.html
