---
title: DEVOPS
date: 2024-08-05 09:59:32
tags:
---

DEV：瀑布模式

DEV+OPS：敏捷开发模式

DEVOPS：敏捷+



需求->功能点



分支命名：YYYYMMDD_DESC_JIRA编号	

测试环境：Noah

noah软路由：提供基准的环境

自动化测试平台：灭霸，对于只读接口的验证。随机找一台线上机器，将流量分发一次，将线上机器和当前机器的结果做一次diff

自动化测试平台：qunit，对于写接口的验证

线上发布：Portal

链路分析：qtrace
