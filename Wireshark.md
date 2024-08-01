---
title: Wireshark
date: 2024-07-17 15:36:12
tags:
---

# 对某个数据包进行分析

## IP层

![image-20240730145129014](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240730145129014.png)

- Identification：数据包的唯一标识
- 标志位：
  - Don't fragment：表示数据包不能被分片
  - More fragments：表示这是数据包的最后一个分片（或数据包没有分片）
  - Fragment Offset：片偏移
- TTL：数据包在被丢弃前可以通过的最大路由器跳数，初始值通常由操作系统设置。
- Header Checksum：用于验证数据包头部的完整性

## TCP层

![image-20240730150308665](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240730150308665.png)

- Stream index：wireshark用于标记一个连接的标识
- Conversation completeness：这个是wireshark提供的会话完整性分析，并不是TCP自带的字段。

# 技巧

RST包意味着重大问题

# 参考文章

https://www.ilikejobs.com/posts/wireshark/
