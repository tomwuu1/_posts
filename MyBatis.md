---
title: MyBatis
date: 2024-07-31 14:01:48
tags:
---

![img](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/mybatis-y-arch-1.png)

插件：在SQL语句的执行中进行拦截。

# 配置

Handler：可以自定义类型处理器

Environments：可以将SQL映射应用于多个数据库

settings：全局的设置

# XML映射器



# 动态SQL标签

<where>标签：可以自动去掉多余的and/or

`<bind>`动态绑定值

# 注意点

1. 一级缓存和二级缓存屏蔽（分布式）
2. 用<![CDATA[ ]]>标记判断条件，放到<where>、<if>里面（有转移字符的）
3. 如判断不是String类型，不要加!=''的条件，否则判断为 false
4. 对于集合类型的返回值，如果没有查找到相关的内容，并不会返回null，而是返回空的集合，但是对于自定义的对象，没有查找到信息时直接返回一个null
5. 使用where id in ()进行查询，没有内容会报错，需要判空，同时size大于0
6. 定义Bean属性用包装类型，防止查询、更新异常（会抛出异常）
7. 

# 转义

< 用`&lt;`代替

![image-20240805174537784](https://kjgadfk-1311071500.cos.ap-nanjing.myqcloud.com/image-20240805174537784.png)
