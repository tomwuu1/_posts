---
title: JaCoCo
date: 2024-08-02 17:17:24
tags:
---

# 圈复杂度

M=E−N+2P

N：node，E：edge，P：连通子图个数

圈复杂度 = 用来覆盖所有可能执行路径的最少测试用例的个数。

javcoco不考虑异常处理



# 工作原理

字节码插桩，两种插桩模式

- on-the-fly：-java agent参数指定特定的jar文件，启动代理程序，在加载类之前判断是否要进行插入
- offline：事先进行插桩
