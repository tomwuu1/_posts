---
title: logback
date: 2024-07-12 13:16:38
tags:
---

# 结构

由logger/appender/layout

- logger 产生日志
- appender 日志输出目的地
- layout将日志格式化

有效日志级别：在程序运行过程中使用的日志级别阈值，小于这个级别的不会传给Appender
# 最佳实践

- logger的additivity属性为false：向上抛，避免重复记录

- 开启异步日志记录，避免日志记录影响业务

- Warn/error级别的日志建议打印入参和处理结果

- 日志保留超过两周

# 日志等级

1. TRACE
2. DEBUG
3. INFO
4. WARN
5. ERROR

# logback中使用占位符拼接变量相较于字符串拼接的好处

- 如果输出的日志级别低于有效日志级别，用占位符的话就不会进行拼接。可读性更强
