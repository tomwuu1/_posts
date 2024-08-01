---
title: Spring
date: 2024-07-17 10:40:26
tags:
---

# IOC

职责边界，不关注不需要关注的事情



@Configuration怎么生效的：生成代理

看背景，看特性，看重要特性

# 容器

问题：怎么统计一个容器的Bean个数，特殊的Bean怎么统计

怎么统计一个容器的Bean个数，特殊的Bean怎么统计：getBeanDefinitionCount：所有的，getBeansOfType特定类型的。

## 扩展

1. 针对某一个Bean
2. 容器级别

# AOP

- 连接点（类，方法，接口）
- 切点：实际的
- advice：增强
- weaving：把代码加入的过程

# 父子容器

父容器中的 Bean 可以被子容器访问，但子容器中的 Bean 不能被父容器访问。

# 两种启动方法的差异

对于传统的大型企业应用，使用外部 Tomcat 服务器可能更合适

# SPI

Service Provider Interface。

框架定义服务接口，具体实现由第三方提供。

Spring SPI/Java SPI



# Spring WebFlux

无锁，全链路异步

- 

