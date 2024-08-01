---
title: RT，并发和QPS之间的关系
date: 2024-07-17 14:22:08
tags:
---

# RT

Response Time（响应时间）

考虑客户端渲染的时间

客户端RT = RTT+ 服务端RT

服务端RT = CPU Time + Wait Time

例子：一个请求10 ms，CPU Time 1 ms，Wait Time 9 ms。单线程的QPS = 100。

理论上最大的单核QPS = 1000 ms / CPU Time = 1000，不过要有10个线程。

再考虑CPU的利用率

![image](https://summer-blog-images.oss-cn-shanghai.aliyuncs.com/qps_optimization/image_003.png)



# 参考文章

https://www.cnblogs.com/huangyingsheng/p/13744422.html?continueFlag=366240e33bd6b19c470b9b48975ab320
