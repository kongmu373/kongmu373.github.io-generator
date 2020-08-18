---
title: "JVM"
date: 2020-07-16T09:27:10+08:00
draft: true
tags: ["面试题"]
series: ["面试题"]
categories: ["面试题"]
---

## JLS 与 JVMS 
+ Java Language Specification(Java语言规范)
  + 定义Java语言的语法
    + (Java编译器)
+ (Java Virtual Machine Specification)Java虚拟机规范
  + 定义字节码如何在JVM中执行
+ 两个没有必然联系(JLS是*.java 编译成 *.class的规范, JVMS是*.class在JVM中运行的规范)

----
### JVM的基本结构
----
![HotSpot JVM Architecture](img/hotspotjvm.png)

+ 堆
  + 所有的对象都在堆上分配
  + 只能操纵对象的引用(或者说是地址)，如:
    `Object obj = new Object();` 其中`obj`只是对象的引用.  
  + 只能操作对象的产生，不能操作对象的销毁
  + 异常:`java.lang.OutOfMemoryError: Java heap space`

+ 方法栈
  + 线程私有
  + 每当一个方法调用发生，方法栈就入栈一个栈帧
  + 每当方法退出(返回或者异常), 栈帧就弹出
  + 因为线程私有，局部变量在栈帧中分配
  + 当栈深度过大时 抛出StackOverflow

+ 栈帧
  + 局部变量表(local variable table)
  + 操作数栈(Operand Stack)

+ 方法区
  + 被整个虚拟机所共享的Class信息(创建对象的说明书)等
  + 运行时常量池
    + 放在常量池的字符串 
      + `str1.intern()`
      + `String str = "abc";`
      + `equals`方法会比较两个引用值是否相等，速度快.
  + Java7及之前: 永久代(PermGen)
  + Java8及之后: 元空间(Metaspace)


### 字节码实现跨平台
+ 字节码(IR, intermediate representation)
  + 一种基于栈的，与平台无关的计算模型
  + 可以由多种方式生成:
    + JVM上的编程语言
    + 直接生成字节码
+ 字节码的加载和执行
![Java运行示意图](https://p0.meituan.net/travelcube/110b593ecf53866e0dec8df3618b0443257977.png)
+ 字节码增强的参考资料:
[字节码增强技术探索](https://tech.meituan.com/2019/09/05/java-bytecode-enhancement.html)

### 类加载器
+ 类加载器大同了代码和数据间的障碍
  + 动态加载代码
  + 动态生成代码
  + 动态替换代码
+ 使用场景
  + Mock
  + AOP
  + 热部署

### 双亲委派加载模型
+ 为什么需要?
  + 安全性 
  + 正确性
    + `instanceof` 也会检查类加载器
+ 越核心的类越是由父亲类加载(Bootstrap ClassLoader  Extension ClassLoader  Application ClassLoader)
![双亲委派模型](https://img-blog.csdn.net/20160506184936657)
