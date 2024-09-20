---
title: Effective Java
date: 2024-09-04 14:05:32
tags:
---

# 1：静态工厂方法（没太懂）

# 2：Builder

这个设计模式主要是为了

1. 在设置复杂参数的时候，可以灵活地设置参数
2. 链式调用有比较好的可读性。
3. 提供可扩展性

就是在一个类里面弄一个Builder，构造函数都返回Builder本身。然后再弄一个方法，返回类本身。

Builder的构造函数里面需要进行参数有效性校验

```java
public class Person {
    private String name;
    private int age;

    public Person(PersonBuilder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }

    public static class PersonBuilder {
        private String name;
        private int age;

        public PersonBuilder setName(String name) {
            this.name = name;
            return this;
        }

        public PersonBuilder setAge(int age) {
            this.age = age;
            return this;
        }

        public Person build() {
            return new Person(this);
        }
    }
}
```

# 3：单例

传统单例模式存在的问题：

- 反序列化破坏单例性
- 反射攻击：通过反射调用构造函数，可以再次创建实例，破坏单例

## 推荐的单例实现方式

声明一个包含单个元素的枚举类型

### 优点

1. 枚举天然支持序列化
2. 反射无法破坏枚举的单例

# 4：私有构造器让类不可实例化

主要是一些工具类，让构造器private。避免被实例化。

# 6：避免创建不必要的对象

1. String
2. Pattern：避免频繁编译正则表达式，缓存Pattern实例
3. 包装类：优先使用基本类型而不是包装类，避免无意识的自动装箱

# 9：用`try-with-resources`

需要类实现`AutoCloseable`接口

```java
try (BufferedReader reader = new BufferedReader(new FileReader("example.txt"))) ;
BufferedReader reader = new BufferedReader(new FileReader("example.txt")))
{
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

1. 简化资源管理，不需要手动关闭资源
2. 使用`try-catch`会吞异常



# 适配器



# 34：枚举

如果我一个方法里面要用一个枚举类型的变量，应该直接传入枚举类型，而不是枚举里面的属性，比如Integer

把一个元素从enum里面删除，没有应用该元素的程序会正常运行，引用了（没有重新编译）的会抛异常

可以在enum定义一个抽象方法的属性，在枚举常量里面覆写这个方法

枚举中的swtich语句不是在枚举中实现特定于常量的行为

# Supplier<T>
