---
title: tc
date: 2024-07-18 11:21:30
tags:
---

# tc命令的原理

## 延迟

把网络包放入缓冲区，记录时间，时间到了送出来

## 带宽

利用令牌桶，tbf -Token Bucket Filter

```sh
sudo tc qdisc add dev veth4dfcb604 root tbf rate 50mbit burst 100kb latency 500ms
```

