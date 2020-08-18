---
title: "Java异常"
date: 2020-05-29T19:41:08+08:00
draft: true
tags: ["异常"]
series: ["异常"]
categories: ["Java体系"]
---


## Java的异常体系
+ Throwable - 可以被抛出的东西(有毒)
  + Exception - checked execption(受检异常，有毒) 预期之内的异常 IOException
    + RuntimeException(运行时异常， 无毒) 预期之外的异常 不应该出现的
  + Error(错误，无毒) OutOfMemoryError 没办法处理的
+ catch的级联与合并
+ 有毒(checked exception)
+ 无毒(unchecked exception)


## try/catch/finally
+ 如果没有try,异常将击穿所有的栈帧
+ catch可以将一个异常抓住
+ fnllay执行清理工作
+ JDK7+: try-with-resource

## throw/throws
+ throw 抛出一个异常
+ throws只是一个声明 (可能抛出一个异常)
  + 调用它的方法都要处理这个异常，或者加throws语句
  + 如果throws的RuntimException 就可以不用

## Throwable
+ 栈轨迹 Stacktrace (排查问题最重要的信息，没有之一)
+ 异常链(Caused by)

## 异常的抛出原则
+ 能用if/else处理的，就不要用异常处理
    + 异常的创建十分昂贵
    + catch到的
    + 不一定是你想要的异常
+ 尽早抛出准异常
  + 如果不能处理就立刻抛出
+ 异常要准确，带有详细的信息
+ 抛出异常也比悄悄地执行错误的逻辑要强得多(不能轻易吞掉异常)

## 异常的处理原则
+ 本方法是否有责任处理这个异常?
  + 不要处理不归自己管的异常
+ 本方法是否有能力处理这个异常?
  + 如果自己无法处理，就抛出
+ 非到万分必要，不要忽略异常

## 了解和使用JDK内置的异常
> 尽量使用内置的异常

+ NullPointerException
+ ClassNotFoundException/NoClassDefFoundError
+ IllegalStateException
+ IllegalArgumentException
+ IllegalAccessException
+ ClassCastException
+ ...
