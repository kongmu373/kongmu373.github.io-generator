---
title: "Item16"
date: 2020-06-21T08:41:51+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 16: In public classes, use accessor methods, not public fields（在公共类中，使用访问器方法，而不是公共字段）
+ 退化类
```java
// Degenerate classes like this should not be public!
class Point {
public double x;
public double y;
}
```
+ 不好的地方:
    - 由于此类的数据字段是直接访问的，因此这些类没有提供封装的好处
    - 不能在不更改API的情况下更改表示形式
    - 在访问或者设置字段的时候不能使用额外的操作
+ 所以应该始终使用private 修饰以及 getter and setter
```java
// Encapsulation of data by accessor methods and mutators
class Point {
private double x;
private double y;
public Point(double x, double y) {
this.x = x;
this.y = y;
}
public double getX() { return x; }
public double getY() { return y; }
public void setX(double x) { this.x = x; }
public void setY(double y) { this.y = y; }
}
```

## 总结
+ public class 不能公开可变 fields
+ public class 可以公开 不可变 fields，但能不用就不用
+ 但有时需要包私有或私有嵌套类公开可变或不可变的字段

