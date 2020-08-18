---
title: "Item1"
date: 2020-06-10T15:42:17+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---



## Item1: 考虑以静态工厂方法代替构造函数

## 什么是静态工厂方法
+ 它是一个类提供的公共静态工厂方法，作用只是一个返回类实例的静态方法。
+ 例子
  ```java
  // boolean 基本类型 转换为 Boolean 对象的引用
  public static Boolean valueOf(boolean b) {
      return b ? Boolean.TRUE : Boolean.FALSE;
  }
  ```
+ 注意的是静态工厂方法与来设计模式的工厂方法模式不同.

### 静态工厂方法的优缺点
+ 相较于构造函数的优点:
  + 静态工厂方法有确切名称
    + 如: 返回可能为素数的BigInteger类它的构造函数为BigInteger(int, int, Random)，静态工厂为BigInteger.probablePrime.
    + 能区分具有相同签名的多个构造函数
  + 静态工厂方法不需要在每次调用时创建新对象
    + Boolean.valueOf(boolean)方法说明了这种技术,重复调用中返回相同对象提高性能
    + instance-controlled classes(实例受控的类),保证它是单例或不可实例化的或不存在两个相同的实例
  + 可以通过静态工厂方法获取返回类型的任何子类的对象
    + 适用于基于接口的框架,其中接口为静态工厂方法提供了自然的返回类型
  + 返回对象的类可以随调用的不同而变化，作为输入参数的函数
  + 当编写包含方法的类时，返回对象的类不需要存在
+ 缺点:
  + 类如果不含共有的或者受保护的构造器，就不能被子类化
  + 名称不固定，程序员很难发现
### 惯用名称
+ from  - 类型转换方法 单参 `Date d = Date.from(instant).` 
+ of  - 聚合方法 多参 `Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);`
+ valueOf -  `BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);`
+ instance 或者 getInstance 由参数来描述实例 `StackWaler luke = StackWaler.getInstance(options);`
+ create或者newInstance 确保每次返回一个新的实例 `Object new Array = Array.newInstance(classObject, arrayLen);`
+ get*
+ new*
+ \*

(注意:*表示类名)

