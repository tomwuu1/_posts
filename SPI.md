---
title: SPI
date: 2024-09-04 17:47:49
tags:
---

# SPI的工作流程

1. 定义服务接口
2. 实现服务接口
3. 在`META-INF/services`中创建文件（文件名是服务接口的全限定名，文件内容是服务实现类的全限定名）用于注册服务提供者

```
com.example.SimpleMessageService
com.example.EncryptedMessageService
```

1. 用`ServiceLoader`加载服务实现

```java
ServiceLoader<MessageService> loader = ServiceLoader.load(MessageService.class);

for (MessageService service : loader) {
    System.out.println(service.getMessage());
}
```



