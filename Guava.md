---
title: Guava
date: 2024-07-12 13:16:38
tags:
---

# Objects

## equal

在判断是否相等的时候，不需要对对象进行null判断

```java
System.out.println(Objects.equal(null, "world"));
```

## hashCode

用于计算一组对象的hashcode

```java
Objects.hashCode(name, age);
```

# MoreObjects

## toStringHelper

帮助实现toString方法

```java
public class Main {
    private String name;
    private int age;

    public Main(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return MoreObjects.toStringHelper(this)
                .add("name", name)
                .add("age", age)
                .toString();
    }

    public static void main(String[] args) {
        Main main = new Main("Hello", 10);
        System.out.println(main.toString());

    }
}
// Main{name=Hello, age=10}
```

## firstNotNull

返回第一个不为null的对象，如果都为null，报异常

```java
System.out.println(MoreObjects.firstNonNull("Hello", "World"));
```



# ComparisonChain

用于使用链式调用实现compareTo方法

```java
public class Person implements Comparable<Person>{
    private String name;
    private int age;
    private int wealth;

    @Override
    public int compareTo(Person other) {
        return ComparisonChain.start()
                .compare(this.age, other.age)
                .compare(this.name, other.name)
                .compare(this.wealth, other.wealth)
                .result();
    }
}
```

# Preconditions

用于进行参数校验。如果没有通过，会抛出异常，并且打印错误信息

## checkArgument

```java
Preconditions.checkArgument(age > 0, "Age must be positive");
```

## checkNotNull

```java
Preconditions.checkNotNull(message, "Message cannot be null");
```

# Ordering

在阅读链式调用的比较器的时候，从前往后读

## natural

```java
List<Integer> numbers = Arrays.asList(5, 3, 9, 1, 4);
Ordering<Integer> naturalOrdering = Ordering.natural();
List<Integer> sortedNumbers = naturalOrdering.sortedCopy(numbers);
// [1, 3, 4, 5, 9]
```

## reverse

返回与原始Ordering相反顺序的Ordering

```java
Ordering<Integer> naturalOrdering = Ordering.natural();
Ordering<Integer> reverseOrdering = naturalOrdering.reverse();
List<Integer> sortedNumbers = reverseOrdering.sortedCopy(numbers);
```

## nullsFirts,nullsLast

用于选择null值的顺序

## onResultOf(Function)

用于提取一个对象需要进行排序的属性

```java
Ordering<Person> byAge = Ordering.natural().onResultOf(new Function<Person, Comparable>() {
    @Override
    public Comparable apply(Person person) {
        return person.age;
    }
});
```

## compound

用于创建多个排序条件，

```java
Ordering<Person> byName = Ordering.natural();

Ordering<Person> byNameThenAge = byName.compound(new Ordering<Person>() {
    @Override
    public int compare(Person p1, Person p2) {
        return Integer.compare(p1.age, p2.age);
    }
});
```

# Throwables

## getRootCause

对于嵌套的异常结构，递归地返回最里面的异常

```java
Throwable rootCause = Throwables.getRootCause(e);
```

# Strings

## isNullOrEmpty/nullToEmpty

## padStart/padEnd/repeat

padStart和padEnd是在字符串的开头或者末尾填充字符，让字符串的长度到达minlen

```java
String s = "k";
System.out.println(Strings.repeat(s, 10));
System.out.println(Strings.padStart(s, 2, '0'));

```

## commonPrefix/commonSuffix

分别用来求两个字符串的最大公共前缀/后缀

# Ints

## compare

判断两个int变量的大小

```java
int a = 0, b = 1;
System.out.println(Ints.compare(a, b));
// -1
```

a < b  -1
a = b 0
a > b 1

## asList

将int数组转换为`List<Integer>`是一个不可变的List，调用add方法会报错

Arrays.asList对于对象数组是正常转成List。对于基础数据类型数组是把整个数组当成一个元素

## contains/max/min

# Joiner

里面不能有null元素，除非用useForNull/skipNulls

```java
String[] words = {"apple", "banana", "cherry", null};        
System.out.println(Joiner.on(",").useForNull("kk").join);
// apple,banana,cherry,kk
String[] words = {"apple", "banana", "cherry", null};        
System.out.println(Joiner.on(",").skipNulls().join(words));
// apple,banana,cherry
```

## map拼接

可以在key和value之间进行拼接

```java
Map<String, String> map = Maps.newHashMap();
map.put("key1", "value1");
map.put("key2", "value2");
System.out.println(Joiner.on(",").withKeyValueSeparator("+").join(map));
```

# Splitter

按照某种模式将字符串进行分割成List或者Map

```java
String s = ",a,b,";
String[] jdkSplits = s.split(",");
System.out.println(Arrays.toString(jdkSplits));

List<String> guavaList = Splitter.on(",").splitToList(s);
System.out.println(guavaList);
// [, a, b]
// [, a, b, ]
```

和String自带的分割区别，jdk会忽略最后的空串

## trimResults

可以接受CharMatcher，如果不填参数就匹配空白字符

```java
String s = ", a + k, b,";
List<String> guavaList = Splitter.on(",").splitToList(s);
List<String> guavaList1 = Splitter.on(",").trimResults().splitToList(s);
List<String> guavaList1 = Splitter.on(",").trimResults().splitToList(s);
System.out.println(guavaList);
System.out.println(guavaList1);
/*
[,  a + k,  b, ]
[, a + k, b, ]
[,  a + ,  b, ]
*/

```

## omitEmptyStrings

去掉空白的元素

```java
String s = ", a + k, b,";
List<String> guavaList = Splitter.on(",").trimResults().omitEmptyStrings().splitToList(s);
System.out.println(guavaList);
// [a + k, b]
```

## 分割成map

```java
String s = "a=1&b=2&c=3";
Map<String, String> map = Splitter.on("&").withKeyValueSeparator("=").split(s);
System.out.println(map);
// {a=1, b=2, c=3}
```

# CharMatcher

有三类方法：匹配char的模式，组合matcher，对于匹配上的char的操作

匹配和处理String中的char

1. anyOf/noneOf/isNot
2. and/or
3. removeFrom/retainFrom

```java
String s = "abcde1234";
String result1 = CharMatcher.anyOf("ab12").and(CharMatcher.is('4')).retainFrom(s);
String result2 = CharMatcher.anyOf("abc").retainFrom(s);

System.out.println(result1);
System.out.println(result2);
/*

abc
*/
```

# Optional

1. 作为返回值，表示返回值可能不存在
2. 设置默认值

```java
Map<Integer, String> map = Maps.newHashMap();
map.put(1, "1");
map.put(2, "1");

int a = Integer.parseInt(Optional.fromNullable(map.get(1)).or(String.valueOf(0)));
int b = Integer.parseInt(Optional.fromNullable(map.get(3)).or(String.valueOf(0)));
System.out.println(a);
System.out.println(b);
/*
1
0
*/
```

# Function

dubbo不支持Guava的List的反序列化？？？？？

用于对象转换

```java
Function<Integer, String> function = x -> x + " ";
List<Integer> numbers = Ints.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
List<String> strings = Lists.transform(numbers, function);
System.out.println(strings);
//[1 , 2 , 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10 ]
```

transform方法的返回值（列表）不支持add方法

## 好处

转换集合用了懒加载的思想，不立刻计算



# Predicate

用于条件判断

```java
Predicate<Integer> greaterThan = i -> i > 1;
List<Integer> numbers = Lists.newArrayList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
Optional<Integer> integerOptional = Iterables.tryFind(numbers, greaterThan);
if (integerOptional.isPresent()) {
    System.out.println(integerOptional.get());
}
// 2
```

# Collections

- binarySearch

```java
for (int i = 0; i < 10; i++) {
    num.add(i);
}
int index = Collections.binarySearch(num, 2);
LOGGER.info("index: {}", index);
```



- sort 排序，可以结合Ordering使用
- shuffle 随机打乱

# Maps

- newHashMapWithExceptedSize
- fromProperties：从properties文件读取生成一个map

# Multimap

可以实现1 -> n关系的构造

# ImmutableXXX

- 保证返回容器不被调用者修改，并且原容器的修改不会影响返回容器
- 返回的对象不是原容器的视图，而是原容器的复制

```java
List<String> list = Lists.newArrayList(Arrays.asList("a", "b", "c"));

ImmutableList<String> immutableList = ImmutableList.copyOf(list);
LOGGER.info("immutableList: {}", immutableList);

list.add("e");

LOGGER.info("immutableList: {}", immutableList);
```

# BiMap

- 让key和value可以互相查询，根据value查询key`BiMap.inverse().get`
- key和value都要是唯一的

# RangeSet/RangeMap

- RangeSet用于存放区间的容器，提供区间合并，区间分裂等功能
- RangeMap，提高Range -> Value的映射，不提供区间合并

# AtomicLongMap 

- 对map里的value进行原子的更新
- 在监控系统里面使用
- addAndGet/getAndAdd

# I/O

## Source and sinks

按照输入/输出，字节/字符分为了四个类
