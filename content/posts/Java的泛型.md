---
title: "Java的泛型"
date: 2020-06-09T18:53:43+08:00
draft: true
---

## 为什么需要泛型?
+ 其实, 以前是没有泛型的
+ 没有泛型写起来就很啰嗦,不同的类型要写出很多除了类型都相同的代码
+ 有了泛型就可以用省力的方法编写类型安全的代码
  + `List<String>`
  + `Map<String, Object>`
  + `Map<String, List<Object>>`

## 泛型存在的问题
+ 向后兼容性
  + https://www.zhihu.com/question/28665443
+ 只有两条路:
  + type erasue(擦除) -> Java的选择
  + 搞一套全新的API -> C#的选择

## Java选择擦除带来新的问题
+ Java的泛型是假泛型，是编译期的泛型
  + 泛型信息在运行期完全不保留
+ 只有编译器的警告
  + 使用限定符List<?>
  + 可以通过利用泛型擦除来绕过编译器检查
+ `List<String>` 并不是`List<Object>`的子类型
  + 类比`String/Object`, `String[]/Object[]`

## 泛型的限定符
+ ? extends 要求泛型是某种类型及其子类
+ ? super 要求泛型是某种类型及其父类
+ Collections.sort 

## 泛型的绑定
+ 泛型方法是如何工作的
+ 泛型方法的绑定
  + 按照参数绑定
  + 按照返回值自动绑定
+ 最难的绑定:
  + Collectors的一系列泛型化方法

## 编写泛型代码
+ 不要一上来就写泛型方法, 而是慢慢重构
+ 编写泛型方法
+ 编写泛型的类

