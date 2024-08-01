---
title: Maven
date: 2024-07-19 16:34:00
tags:
---

maven版本 3.3.9

# 依赖

## 版本



SNAPSHOT：不稳定，用于开发

RELEASE：正式版本

PRE：灰度版本

## 环境(profile)

dev 

## 依赖管理

dependecyMangement进行显式管理

## 如何写好pom

- 相关联的多个包版本一致，使用一个变量定义
- 直接依赖的包要显式引入，不能间接引入。

# 父子工程

- 父pom只规定版本，不进行引入。子pom只引入，不规定版本
