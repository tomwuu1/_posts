---
title: PostgreSQL
date: 2024-07-30 10:02:14
tags:
---

# 优点

- 丰富的数据类型
- 强大的对应用透明的原生表分区功能
- 开源里最好的地理信息插件PostGIS
- 外部数据源支持
- Online DDL（无论加字段的表多大及是否带not null default，都秒级完成）



# 安装

用docker安装

```sh
docker pull postgres
docker run --name my_postgres -e POSTGRES_PASSWORD=123456 -d postgres
docker exec -it my_postgres psql -U postgres

```

# psql常用命令

\h：help命令

\l：查看数据库列表

# 命名规范

1. 长度不能超过63个字符
2. 不建议以pg开头，不建议以数字开头
3. 禁止使用SQL关键字

图片



# 字段设计

1. 用varchar(n)而不是char(n)
2. 使用default null（占用1bit），而不用default ''（32bit)
3. 使用timestamp with time zone，支持业务国际化
4. 使用numeric（precision,scale）
5. 用hstore存储key-value类型，对数不定的
6. 用ltree来存储树状结构
7. 用jsonb（比json更好，json是基于对于输入的解析），jsonb是解析完的二进制。jsonb可以创建索引
8. 用Geometric Types结合PostGIS对于地理信息存储
9. 用range类型实现范围存储
10. 在给date,timestamptz设置default值的时候用now()
11. 

# 索引设计

1. 加删/索引，加CONCURRENTLY参数，提升并发效果

# 关于NULL

1. NULL的判断：IS NULL
2. boolean类型取值true,false,NULL
3. 对字符串类型/hstore ，对NULL值处理之后用 ||处理 ？？？
4. 

# 开发规范

1. 避免select *，
2. 大批量数据入库用copy，不用insert
3. 
