---
title: 模拟Buffer对于传输的影响
date: 2024-07-15 14:18:45
tags:
---

# 实验设置

1.调整接收buffer大小

原来的参数

```sh
4096	16384	4194304
```

新的参数

```sh
sudo sysctl -w "net.ipv4.tcp_rmem=16384	16384	  16384"
```

现象11s才跑完，之前是9s

再调大一些

```sh
sudo sysctl -w "net.ipv4.tcp_rmem=46384	46384	  46384"
```

速度稍微快了一些

调到最大试试

```sh
sudo sysctl -w "net.ipv4.tcp_rmem=4194304	4194304	  4194304"
```

发现速度快了很多就跑完了。

那就调整wmem试试看

```sh
//默认参数
net.ipv4.tcp_wmem = 4096	16384	4194304

sudo sysctl -w "net.ipv4.tcp_wmem=4194304	4194304	  4194304"
```

# 
