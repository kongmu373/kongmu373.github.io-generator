---
title: "HashMap源码分析"
date: 2020-06-08T10:09:46+08:00
draft: true
tags: ["HashMap"]
series: ["数据结构"]
categories: ["java8体系"]
---

## 常见面试问题
+ HashMap默认容量?
+ HashMap如何扩容?
+ HashMap的数组大小为什么一定要是2的幂?
+ HashMap为什么是线程不安全的
+ Java 7到8做了哪些改进? 为什么?
+ ...

## 哈希表的简介
+ 核心是基于哈希值的桶和链表
  + 将一个元素映射成一个便于搜索的哈希值
+ O(1)的平均查找,插入 删除时间
  + 桶底层使用数组实现，数组查找哈希值便是O(1)
  + 而插入元素是在链表或树中插入的，所以插入/删除也是O(1)
+ 致命缺陷是哈希值的碰撞(collision)

## Java 7 HashMap
+ 经典的哈希表实现: 数组+链表
+ 重要知识点:
  + 初始容量
    + `DEFAULT_INITIAL_CAPACITY = 1<<4; // aka 16`
    + 为什么 Must be a power of two?
      + `newTab[e.hash & (newCap - 1)] = e;`
      + newCap是2的幂 - 1 得到 二进制全部是111...,让它与某个元素进行&操作，得到数组下标。
      + 如果length不是2的幂，那么 newCap - 1,二进制得到的就不全是111...了，不是0的位就不可能分配得到元素,整个数组就没有得到平均分配。
  + 负载因子
    + `DEFAULT_LOAD_FACTOR = 0.75f;`
  + 哈希算法
  + 哈希桶
    + `transient Node<K,V>[] table;`
    + 数组
  + 扩容
    + 大于 容量 * 负载因子 就开始扩容 (16*0.75=12 超过12就开始扩容了)
    + 低效
    + 线程不安全  
  + 并发环境中易死锁
    + 死锁问题分析
      + (resize中的transfer，发生链表死循环)
  + 可以通过精心构造的恶意请求引发DoS
    + 链表性能退化分析
  + ReSize效率很低
    + 根据问题规模调节初始容量

## Java8 HashMap的改进
+ 数组+链表/红黑树
+ 扩容时插入顺序的改进
+ 函数方法
  + forEach
  + compute系列
+ Map的新API
  + merge
  + replace 
