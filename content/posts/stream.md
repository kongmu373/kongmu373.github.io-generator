---
title: "Stream"
date: 2020-06-07T14:33:15+08:00
draft: true
tags: ["Java8"]
series: ["Java8"]
categories: ["Java体系"]
---

## 什么是Stream
+ 一个 "流"
+ 好处:
  + 不容易出错
  + 简化代码
  + 可读性/可维护性++
```
// 请把姓张的用户挑出来，把他们按照年龄排序,然后把它们的名单报告给我    
List<User> users = getUsers();          
List<String> list = users.stream().
        filter(user -> user.name.startsWith("张"))
        .sorted((Comparator.comparing(User::getAge)))
        .map(User::getName)
        .collect(Collectors.toList());
```
+ 使用idea的plugin Java Stream Debugger来了解流


## 创建一个Stream
+ Stream的API
  + Collection.stream()
  + Stream.of
  + String.chars()
  + IntStream.range()
```java
IntStream.range(0, 11)
        .filter(i -> i % 2 != 0)
        .mapToObj(i -> "," + i)
        .forEach(System.out::println);
```
## Stream的中间操作
+ 返回Stream的操作
+ Stream的API:
  + filter
  + map
  + sorted

## Stream的终止操作
+ 返回非`Stream` 的操作, 包括void 
+ 一个流只能被消费一次
+ forEach
+ count/max/min
+ findFirst/findAny
+ anyMatch/noneMatch
+ *collect*
```java
public static int countUpperCaseLetters(String str) {
    return str.chars().filter(ch->Character.isUpperCase(ch))
    .count();
}

// 去看看用户里面有没有人姓李，把姓李的人挑出来
Boolean anyMatch = users.stream().anyMatch(user->user.getName.startsWith("li"));
```

## collector与Collectors
+ collect操作是最强大的操作
+ toSet/toList/toCollection
+ joining()
+ toMap()
+ groupingBy()
+ 查看Collectors.java中注释的例子
## 并发流
+ parallelStream()
```java
public static int howManyPrimeNumbers(int n) {
    IntStream.range(1,n).parallel().filter(Primes::isPrime).count();
}
```

## 参考文献
Effective Java Item 42-48