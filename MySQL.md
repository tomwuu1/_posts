---
title: MySQL
date: 2024-07-29 11:22:37
tags:
---

表字符集，无特殊情况请一律使用utf8mb4

# DDL

```sql
TRUNCATE TABLE table_name;
```

比delete快

alter修改表的结构



# 如何查看表的结构信息？如何查看索引选择性？

```sql
DESCRIBE author;
```



# 存储引擎

![image-20240729132725994](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240729132725994.png)

## 物理存储结构

- MyISAM

`.frm` 文件：存储表的结构定义。

`.MYD` 文件：存储表的数据（MyISAM Data）。

`.MYI` 文件：存储表的索引（MyISAM Index）。

- Innodb

`.frm` 文件：存储表的结构定义。

`.ibd` 文件：存储表的数据和索引（InnoDB Data）。

# 字段类型

整形括号里面的数字不影响范围

decimal（总共的位数，小数的位数），是准确的数字

## 字符类

- BLOB：二进制字符串
- VARCHAR：

## 日期

TIMESTAMP：2038年，推荐使用DATETIME

![image-20240729140215489](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240729140215489.png)

# 权限校验流程

**全局权限**：检查 `mysql.user` 表中的全局权限。

**数据库级权限**：如果全局权限不足，检查 `mysql.db` 表中的数据库级权限。

**表级权限**：如果数据库级权限不足，检查 `mysql.tables_priv` 表中的表级权限。

**列级权限**：如果表级权限不足，检查 `mysql.columns_priv` 表中的列级权限。

# 创建用户

```sql
create user 'username'@'host' IDENTIFIED BY "password";
```

## 赋予权限

```sql
grant select, insert, update on database_name.table_name to 'username'@'host';
```

## 删除权限

```sql
 revoke select on database_name.table_name to 'username'@'host';
```

![image-20240729141849229](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240729141849229.png)

## 权限安全

密码：

- 16个字符，大小写字母+数字，字母开头

# show

## show create

```sql
show create table table1;

show create database database1;
```

## show table status

```sql
show table status likef 'table1'\G
```

`\G`纵向显示

## show index

```sql
show index from table1\G
```

## show grants

```sql
show grants for 'username'@'host'\G
```

## show processlist

查看当前连接的用户以及执行SQL（会影响性能）

```sql
show full processlist\G
```

没有full时，如果结果过长只会打印部分

# set

用于修改服务器的会话变量或全局变量

# 命名规范

## 库表

1. 库名、表名必须使用小写字母，并采用下划线分割。
2. 库名、表名禁止超过32个字符。
3. 库名、表名必须见名知意。命名与业务、产品线等相关联。
4. 库名、表名禁止使用MySQL保留字。（保留字列表见官方网站）
5. 临时库、表名必须以tmp为前缀，并以日期为后缀。例如tmp_test01_20130704.
6. 备份库、表必须以bak为前缀，并以日期为后缀。例如bak_test01_20130704。

## 字段

1. 字段名必须使用小写字母，并采用下划线分割，禁止驼峰式命名
2. 字段名禁止超过32个字符。
3. 字段名必须见名知意。命名与业务、产品线等相关联。
4. 字段名禁止使用MySQL保留字。（保留字列表见官方网站）

## 索引

1. 索引名必须全部使用小写字母，并采用下划线分割，禁用驼峰式。
2. 非唯一索引按照“idx_字段名称”进用行命名。例如idx_age_name.
3. 唯一索引按照“uniq_字段名称”进用行命名。例如uniq_age_name.
4. 组合索引建议包含所有字段名，过长的字段名可以采用缩写形式。例如idx_age_name_add.

## 业务命名规范

![image-20240729144537517](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240729144537517.png)

![image-20240729144700934](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240729144700934.png)

# 基础规范

1. 使用 INNODB 存储引擎并且使用业务不相关的自增 ID 作为主键。
2. 表字符集使用 UTF8MB4 字符集。
3. 所有表、字段都需要添加注释。推荐采用英文标点，避免出现乱码。
4. 禁止在数据库中存储图片、文件等大数据。
5. 每张表数据量建议控制在 5000 万以内。禁止在线上做数据库压力测试。
6. 禁止从测试、开发环境直连数据库。
7. 不能使用存储过程、触发器等PLSQL

date类型不让用

# 索引规范

1. 单张表中索引数量不超过5个。
2. 单个索引中的字段数不超过5个。
3. 索引名必须全部使用小写。
4. 组合索引建议包含所有字段名，过长的字段名可以采用缩写形式。例如
   idx_age_name_add.
5. 表必须有主键，推荐使用unsigned自增列作为主键。
6. 唯一键由3个以下字段组成，并且字段都是整形时，可使用唯一键作为主
   键。其他情况下，建议使用自增列或发号器作主键。
7. 禁止使用外键。
8. 禁止冗余索引
9. 联表查询时join列的数据类型必须相同，并且要建立索引。
10. 不在低基数列上建立索引，例如“性别”。
11. 选择区分度大的列建立索引。组合索引中，区分度大的字段放在最前。
12. 对字符串使用前缀索引，前缀索引长度建议不超过8个字符，需要根据业务实际需求
    确定。
13. 不对过长的VARCHAR字段建立索引。建议优先考虑前缀索引，或添加CRC32或
    MD5伪列并建立索引。？？？
14. 合理创建联合索引，(a,b,c)相当于(a)、(a,b)、(a,b,c).
15. 合理使用覆盖索引减少IO,避免排序。
16. order by后面的字段尽量也要放在索引之中

# 字符集规范

1. 同一个实例的库表字符集必须一致，JOIN字段字符集必须一致
2. 禁止在字段级别设置字符集
3. 字符集更改，`alter table t1 convert to charset utf`

# 常见问题

- 如何存储IPV4/IPV6：unsinged int 存储IP地址
- INT(1)：1表示显示字符宽度，非存储长度

# 库表设计

1. 将大字段、访问频率低的字段拆分到单独的表中存储，分离冷热数据
2. 表的默认字符集指定UTF8MB4（特殊需求除外），无须指定排序规则
3. 主键用整数类型，并且字段名称用id,使用AUTO INCREMENT数据类，并指定UNSIGNED

# 分表策略

1. 推荐使用HASH进行散表，表名后缀使用十进制数，数字必须从0开始
2. 按日期时间分表需符合YYYYMMHH格式，例如2017011601。年份必须用4位数字表示。例如按日散表user_20170209、按月散表user_201702
3. 采用合适的分库分表策略。例如千库十表、十库百表等

# 库表禁止使用项

1. 禁止以非字母开头命名表名及库名
2. 禁止使用分区表

# 字段设计

1. 建议使用UNSIGNED存储非负数值
2. 所有字段均定义为NOT NULL
3. 用DECIMAL代替FLOAT和DOUBLE存储精确浮点数。例如与货币、金融相关
   的数据
4. 区分使用TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT数据类型
5. 使用VARBINARY:存储大小写敏感的变长字符串或二进制内容
6. 像是分类，语言等可以被枚举尽的，可以使用tinyint

# 字段禁止使用项

1. 建议尽可能不使用ENUM类型替代TINYINT
2. 建议尽可能不使用TEXT、BLOB类型（行溢出）

# 用不到索引

- 否定条件：!=， not in ，not exists
- join中连接字段类型/字符集不一致

# GTID

server_id+transaction_id

## 优点

- 简化故障转移
- 简化日志管理

# 避免bad sql

- delete from改成 truncate table
- select/update范围大，改成分段做，一定要有索引可走
- 避免子查询（性能太差，可读性太差）
- 禁止select *



- 避免出现大事务（容易出现锁等待超时）
- 事务里面不能夹杂RPC(会出现数据不一致情况)
- Spring 事务传播级别为PORPAGATION_REQUIRES_NEW时，子事务不能操作主事务锁住的记录
- 增删改查数据量较大时，要和DBA协商
- 统计信息类SQL一定要离线库跑

# 半同步复制

主库将binlog发送给所有配置为半同步复制的从库。接收到至少一个从库的ack后，才会返回客户端确认事务提交成功

# MySQL集群架构

## 3M架构

应用程序通过VIP连接

![image-20240730222938002](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240730222938002.png)

缺点：不能跨机房（VIP），不能做多线程复制，moniter单点

## qmha

![image-20240730222920715](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240730222920715.png)



## PXC架构

节点对等，多写。namespace，

如果同一个集群节点多了，会影响性能

![image-20240730222330013](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240730222330013.png)

切换更流畅

# pt-osc

# 未分类

- 要进行连表操作的id是要独立设置一个id，不能使用表的自增id吗？
- 数据表的更新时间字段是用CURRENT_TIMESTAMP 还是以服务器的时间为准？
- 连表不用数据库，在java程序里面操作。
- 一个表有唯一索引，怎么做逻辑删除：把唯一索引字段和逻辑删除字段做联合索引，逻辑删除就把逻辑删除字段设置为null
- 索引长度限制767字节
- 大小写敏感：默认是大小写不敏感的，需要设置排序规则，比如`COLLATE` `= utf8mb4_bin`
