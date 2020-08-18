---
title: "Item15"
date: 2020-06-20T17:10:08+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 13: Minimize the accessibility of classes and members(尽量减少类和成员的可访问性)


## 封装
+ 封装的概念：
  + 一个经过精心设计的模块将隐藏其所有实现细节，从而将其API与实现完全分开
  + 然后，模块仅通过其API进行通信，而忽略了彼此的内部工作原理
+ 为什么使用封装:
  + 它使构成系统的模块脱钩，从而使它们可以独立开发，测试，优化，使用，理解和修改
  + 并行开发模块，因此可以加快系统开发速度
  + 更快速地理解和调试模块，而几乎不担心会损坏其他模块，因此它减轻了维护负担

## 使每个类或成员尽可能不可访问
+ 对于顶级（非嵌套）类和接口:
  + 只有两种可能的访问级别：包私有和公共
  + 如果可以将顶级类或接口设置为包私有，则应该这样做
  + 如果成为程序包私有，就不必担心会损害现有客户端
  + 如果将其公开，则有义务永远支持它以保持兼容性

+ 对于成员（字段，方法，嵌套类和嵌套接口）
  + 有四个可能的访问级别：private, package-private, protected, public 
  + 在为class设计公开的API后，将其他所有成员设为`private`,仅当同包有需要访问成员才删除某个`private`
  + 实例成员不应该设置为public 
  + 将可变成员设为public是线程不安全的，甚至将reference为final以及它的指向也设为不可变对象也不应该
  + 注意,非空的数组总是可变的，将该成员设置为public static final 肯定是错误的
```java
// Potential security hole!
public static final Thing[] VALUES = { ... };

// fix problem.
// 1.
private static final Thing[] PRIVATE_VALUES = { ... };
// 2.
public static final List<Thing> VALUES =
Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));

// Alternatively, you can make the array private and add a public method that returns a copy of a private array:
private static final Thing[] PRIVATE_VALUES = { ... };
public static final Thing[] values() {
return PRIVATE_VALUES.clone();
}
```

## 总结
+ 应该始终尽可能减少可访问性
+ 应防止任何杂散的类，接口或成员成为API的一部分
+ 除公共静态最终字段外，公共类不应具有公共字段
+ 确保公共静态最终字段引用的对象是不可变的