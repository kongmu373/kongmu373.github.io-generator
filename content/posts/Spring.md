---
title: "Spring"
date: 2020-06-12T08:57:48+08:00
draft: true
tags: ["Spring"]
series: ["Spring"]
categories: ["Java体系"]
---

## Spring是什么?
+ Java世界应用的事实标准
  + Spring容器 - 一个 IoC容器
  + Spring MVC - 基于Spring和Servlet的Web应用框架
  + Spring Boot - 集成度和自动化程序更高


## 没有Spring,我们会怎么做呢?
+ 选择一: 一个main程序打天下
  + 非常轻量，适用于非常简单的场景
+ 一旦规模上来之后
  + 难以维护
  + 是一场灾难
+ 选择二: 拆分并手动管理
  + 拆分成多个模块
  + 优点:
    + 方便测试
    + 方便共同开发
    + 方便维护
  + 缺点:
    + 依赖关系纷繁复杂

## Spring容器的核心概念
+ Bean
  + 容器中的最小工作单元，通常为一个Java对象
+ BeanFactory/ApplicationContext
  + 容器本身对应的java对象
+ 依赖注入 (DI)
  + 容器负责注入所有的依赖
+ 控制反转(IoC)
  + 用户将控制权交给了容器
