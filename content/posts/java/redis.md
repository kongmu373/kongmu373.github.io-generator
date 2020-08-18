---
title: "Redis"
date: 2020-07-05T09:53:34+08:00
draft: true
tags: ["redis"]
series: ["Java体系"]
categories: ["Java体系"]
---

## 分布式与redis
+ 当服务器不止一台的时候，就不能将会话状态信息放在服务器的内存里了
+ 用redis集中存储状态信息，就可以在分布式中维持登录状态信息. 
![系统结构图](https://kongmu37301.oss-cn-shenzhen.aliyuncs.com/%E7%B3%BB%E7%BB%9F%E7%BB%93%E6%9E%84%E5%9B%BE.jpg)

## Redis为什么这么快?
+ redis相当于一个HashMap
+ 运行在内存中(因为运行在内存中相对于MySQL就很贵了)
+ IO多路复用机制
+ 单线程
  + 避免上下文切换的开销
  + 避免锁的开销
![程序](/img/内存.jpg)
+ 操作系统的基本原理
  + CPU是时钟驱动的，基本执行单位时指令
  + 指令来自于程序的代码段
  + CPU读取程序的代码段,挨个执行指令
    + 可以从内存和寄存器交换数据
    + 可以执行计算
    + 可以跳转
  + 某一个时刻只有一个进程/线程占用CPU,执行指令
+ 上下文切换
  +  CPU需要:
     +  保存现场: 正在执行的任务所有的寄存器
     +  恢复线程: 即将被执行的任务所有的寄存器
  +  速度:
     +  数千个时钟周期

## IO多路复用
+ 传统的IO
  + 如何从操作系统的IO中读写数据的?
    + recv/send系统调用 阻塞的
    + 类比Java的Socket
```java
FileInputStream fis = new FileInputStream("");
// 是个阻塞操作，总个线程就会等待在那里，直到获取到数据
fis.read();
```
+ IO多路复用就是一个线程能处理成千上万的IO事件
  + select/poll/epoll

## IO模型
+ Blocking IO (不占用CPU，跟死循环占100%CPU不同)
  + recv阻塞至有数据返回
+ Non-blocking IO 
  + 如果无数据，recv立即返回，给出一个状态码

## 多路IO 
+ 同时监控多个socket
+ select/poll
  + 监控多个file descriptor
  + 功能几乎相同，公用代码
  + 当返回时，需要轮询FD列表检查是否有事件发生
  + select可能轮询一个稀疏的集合
+ epoll
  + 当返回时，只有有事件发生的FD会被返回
  + 不需要轮询
  + 阻塞到有事件产生

## reids基础
![redis](https://kongmu37301.oss-cn-shenzhen.aliyuncs.com/redis%E5%9F%BA%E7%A1%80.jpg)
