---
title: "Java包管理"
date: 2020-05-25T14:29:03+08:00
draft: true
tags: ["包管理"]
series: ["Maven"]
categories: ["Java体系"]
---


## 包是什么?
   +  就是把许多类放在一起打的压缩包
  

## 什么是包管理
+ 包管理的本质就是告诉JVM如何找到所需的第三方类库

## 为什么要用包管理呢？
从运行一个JAVA程序说起：
   1. JVM的工作内容:
      + 执行一个类的字节码
      + 加入这个过程中碰到了新的类， 加载它
      + 那么，去哪里加载这些类呢?(通过类路径) 
   2. 类路径 (Classpath)
      + 在哪可以找到Classpath
        +  -classpath/-cp
      + 类的全限定类名(目录层级) 唯一确定了这一个类

   3. 传递依赖
      + 你依赖的类还依赖了别的类
      + Classpath hell
        + 全限定类名是类的唯一标识
        + 当多个同名类同时出现在Classpath中,就是噩梦的开始
 
也就是说，当JVM加载越来越多的类时，仅用人手一个一个去添加jar包就变得不切实际了，必须用到包管理工具。


## 启蒙时代
+ Apache Ant
  + 手动下载jar包，放在一个目录中
  + 写XML配置，指定编译的源代码目录，依赖的jar包，输出目录等
+ 缺点
  + 每个人都要自己造一套轮子
  + 依赖的第三方类库都需要手动下载，费时费力
    + 假如你的应用依赖了一万个第三方的类库呢？
  + 没有解决Classpath地狱


## Maven -- 划时代的包管理
+ Convention over configuration (约定优于配置)
+ 必须强调, Maven 远远不止是包管理工具
+ Maven 的中央仓库
  + 按照一定的约定存储包
+ Maven的本地仓库
  + 默认位于 ~/.m2
  + 下载的第三方包放在这里进行缓存
+ Maven的包
  + 按照约定为所有的包编号, 方便检索
  + groupId/artifactId/version
    + 扩展: 语义化版本 (x.y.z) (x->大版本，y->小版本, z->修bug)
  + SNAPSHOT版本 (开发版本)

  
## Maven -- 包冲突及解决实战
+ 传递性依赖的自动管理
  + 原则: 绝对不允许最终的classpath出现同名不同版本的jar包
+ 依赖冲突的解决
  + 原则：
    + 分支不同时,最近的胜出.
    + 分支相同时,靠前的胜出.
  + 解决方案一: 直接在dependencies 引入你想要的包
  + 解决方案二: 
  ```xml
  <dependency>
    ...
    <exclusions>
        <exclusion>
            ...(排除不想引入的包 不需要版本号)
        </exclusion>
    </exclusions>
  </dependency>
  ```
+ 依赖的scope (依赖的隔离)
  + compile (在main和test中都有效)
  + test (只在test中有效)
  + provided (只在编译有效，运行就没有效果了)
    + 程序一般先编译，再运行，而provided 只在编译过程时发生效果 
  
+ 常见的冲突报错信息
  + AbstractMethodError
  + NoClassDefFoundError
  + ClassNotFoundException
  + LinkageError

##  Maven -- 实战哪些章节是值得看的?
  + 5，6，7，8章(坐标和依赖/生命周期/仓库/聚合和继承)  
