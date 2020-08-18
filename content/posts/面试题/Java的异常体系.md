---
title: "Java的异常体系"
date: 2020-07-05T09:50:22+08:00
draft: true
tags: ["面试题"]
series: ["面试题"]
categories: ["面试题"]
---

## Java的异常体系结构?
+ Throwable，异常/错误的祖先类,任何可以被丢出来的东西
  + throws Throwable的子类
  + Error extends Throwable
    + 致命的错误
  + Exception extends Throwable
    + 可以从异常状态中恢复
    + RuntimeException extends Exception 预料之外的异常,通常代表一个bug
      + unchecked Exception
      + 不用被编译器所检查,其他异常都需要编译器检查(checkedException)
      + 常见子类:
        + NullException
    + 其他Exception 预料之中的异常,代表一个预期状态(checked)
## 什么是checked/unchecked/runtime exception?
+ checked是预料之中的异常，是一定需要捕获的
+ unchecked是预料之外的异常，通常代表一个bug，可以不需要捕获
+ unchecked的类名是runtime exception

## try/catch/finally 执行顺序
先执行try语句的内容，如果没有任何异常就执行finally,如果有异常发生并能被catch语句捕获，就会阻止try语句后面的执行，跳到catch语句，然后执行finally.
```java
try{

}catch (xxxx e) {

} finally() {

}
```
+ try执行return,finally也会在return前执行 

## throw/throws的区别
+ throw 
    + 是一个语句，丢出一个异常，阻止程序的正常执行
        `throw new RuntimeException()`

+ throws
  + 是一个方法的签名
    + 声明该方法会调一个异常

## final/finally/finalize的区别
+ final
  + 声明在类上，表示该类不可被继承的
  + 声明在方法上，表示该方法是不可被覆盖的
  + 声明在变量上，表示该变量不可被修改的(引用类型就是不可被改变指向)
+ finally 
  + 用于try-catch中最后的保险开关
+ finalize
  + 在垃圾回收的时候，由垃圾回收器调用
  + 不要手工去调用它


