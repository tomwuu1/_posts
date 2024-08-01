---
title: Redis
date: 2024-07-31 18:39:02
tags:
---

# 集群架构

## cluster

![](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240731183942723.png)

优点：动态扩容

缺点：存储Slot-Key关系占用较多内存，Key迁移是阻塞。

## Proxy

![image-20240731184234674](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240731184234674.png)

优点：解耦Client访问逻辑和Redis分片逻辑

缺点：Proxy中转存在性能损耗

![image-20240731184536459](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240731184536459.png)

优点：直接访问Redis，性能高。支持动态扩容。

zookeeper：SDK服务注册中心，Redis实例变更通知中心

Sentinel集群：故障之后负责更新Zookeeper和配置中心

# 规范

1. 避免bigkey。string < 10KB。hash,list,set,zset元素个数<5000.
2. 避免直接删除bigkey。
3. 
