---
title: "GC"
date: 2020-08-04T10:46:11+08:00
draft: true
categories: ["jvm"]
---

## GC
1. 什么是垃圾?
   + 没有任何引用指向的一个对象或者一个对象组(循环引用),都是垃圾 
   + C/C++ : malloc/new free/delete
   + Java : new ?(自动内存回收)
     + 优点: 编程更简单，减少人工的出错问题(忘记回收--内存泄漏，多次回收--可能删了有价值的东西)

2. 怎么找到这些垃圾呢?
   + reference count
     + 不能解决循环引用  
   + Root Searching(根可达算法,通常使用的算法)
       + 例如，线程栈变量,静态变量，常量池的变量,JNI指针 都是 GC roots
> Which instance are roots?
>   JVM stack, native method stack, runtime constant pool, static references in method area, Clazz

3. 怎么回收这些垃圾呢?
   + Mark-Sweep (标记清除) 
     + 位置不连续
     + 产生碎片 
   + Copying (拷贝)
     + 没有碎片
     + 浪费空间，效率不高
   + Mark-Compact (标记压缩)
     + 不浪费空间，没有碎片
     + 但是比Copying效率更低

4. JVM分代算法
   + new - young
     + 存活对象少
     + 使用copy算法
   + old
     + 垃圾少
     + 一般使用mark compact
   + 新生代 + 老年代 + 永久代(7)/元数据(8)
     + 永久代，元数据 - 存储对象的说明书(Class)
     + 永久代必须指定大小限制, 元数据可以设置，也可以不设置，无上限(受限于物理内存)
     + 字符串常量(7)存储在永久代,(8)存在堆上
   + young = eden(8) + (1)survivor + (1)survivor 
     + 进行一次 YGC回收之后，大多数的对象都会被回收，活着的进入 s0
     + 再次YGC, 活着的对象 eden + s0 -> s1
     + 再次YGC,  eden + s1 -> s0
     + 年龄足够 -> 老年代
     + s区装不下 -> 老年代
   + old
     + FGC, Full GC
   + GC Tuning(Generation)
     + 减少Full GC的频率

5. 常见的垃圾回收器
   1. 年轻代
      1. Serial 年轻代 串行回收
      2. Parallel Scavenge 并行回收   
      3. ParNew 配合CMS的并行回收
   2. 老年代
      1. Serial Old
      2. Parallel Old
      3. ConcurrentMarkSweep Old CMS 并发的
   3. G1
   4. ZGC
   5. ...
> java8默认的垃圾回收: PS + ParallelOld

6. 怎么拿到java的垃圾回收器是什么呢?
  + java -XX:+PrintCommandLineFlags
  + java -XX:PrintFlagsFinal 
  + java -XX:PrintFlagsInitial

