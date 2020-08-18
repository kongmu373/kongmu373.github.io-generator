---
title: "Spring原理"
date: 2020-06-18T16:41:11+08:00
draft: true
tags: ["spring"]
series: ["spring"]
categories: ["spring"]
---

## 为什么需要Spring呢？
+ 开发一个应用要把它分成很多个模块
+ 模块之间用约定进行通信
+ 模块分成很多个子模块
+ 如果是java应用程序，那么模块可以理解为一个个对象(Object,在Spring中命名为Bean)
+ 模块之间存在相互依赖,如：当A模块必须使用B模块才能完成A模块提供的服务(A依赖B)
+ 当这个应用里的模块不断相互依赖时，它们之间的依赖关系就很难用人手去维护了，所以就需要Spring了。

## 什么是Spring呢?
+ Spring是Java大型应用开发的事实标准
+ Spring是一个容器
+ 容器里有无数个bean(提供一个服务的对象)
+ 这个容器通常叫做IoC容器
+ Spring的IoC(Inverse of Control, 控制反转)与DI(Dependency Injection, 依赖注入)
  + IoC
    + 这些模块依赖的装配控制权由程序员反转给Spring,就是说只需告诉Spring模块之间的依赖关系,就能完成装配了
  + DI
    + 就是A依赖B,就像有个针头把B注入到A模块中，与IoC的概念一样
