---
title: "Javase 1"
date: 2020-08-01T09:35:47+08:00
draft: true
categories: ["Java"]
---

## Java 与 C/C++
+ 面向过程: C语言面向过程,主要关注的是数据的流向
+ 面向对象: C++和Java都是面向对象. 主要关注的是不同对象之间的交互
+ Java把C++的复杂语法，手动释放内存，以及容易造成编程错误的指针等弊端都屏蔽了
+ Java与操作系统之间还有一个JVM作为中间件，以至于实现跨平台，可移植，垃圾回收等等

## Java运行条件
+ JVM: JAVA virtual Machine java虚拟机
+ JDK: JAVA Develop Kit JAVA开发工具
+ JRE: JAVA Runtime environment JAVA运行环境

## 类加载器
1. 加载
2. 验证
   + class文件的版本能不能兼容当前JVM版本
   + class文件是否满足JVM规范(class的第一个字节码文件为魔术值)
3. 准备
   + 把类成员初始化为系统默认的初始值 [final的类变量除外,final变量直接初始化为变量值] 
     + 如: byte 0 , short 0, int 0, long 0L, char \u0000, boolean false, float 0.0f, double 0.0d,引用类型 null
4. 解析
   + 把符号引用解析为直接引用
   + 符号引用: 就是我们写的xx变量 如 Integer xx = new Integer();
   + 解析就是把xxx,这招符号引用变为直接引用即内存地址  
5. 初始化
   + 把我们定义的static变量或者static静态代码块按顺序组织成<clinit>构造器(也称作类构造器)来初始化变量
6. 使用
   + 加载实例信息进入开辟的内存中
   + 执行构造方法就是<init>方法 
7. 卸载

## 类创建过程
```java
class Father {
    static {
        System.out.println("父类静态代码块");
    }
    {
        System.out.println("父类普通代码块");
    }

    public Father() {
        System.out.println("父类构造方法");
    }
}

class Son extends Father {
    static {
        System.out.println("子类静态代码块");
    }
    {
        System.out.println("子类普通代码块");
    }

    public Son() {
        System.out.println("子类构造方法");
    }
}

public Demo {
    public static void main(String[] args) {
        Son son = new Son();
        /*
         sout:
         父类静态代码块
         子类静态代码块
         父类普通代码块
         父类构造方法
         子类普通代码块
         子类构造方法
        */
    }
}
```

## 面向对象 (关注的是对象之间的交互)
> OOA,OOD,OOP
+ OOA: 面向对象分析
  + 分析出这里有几个对象,对象含有的动作，属性
+ OOD: 面向对象设计
  + 对OOA出来的对象进行对象之间的交互设计
+ OOP: 面向对象编程
  + 对OOD出来的对象之间的交互关系进行编码 
1. 封装
   + 对对象封装一层，只提供外部访问的方法,保护内部数据
2. 继承
   + 方便代码量的书写,继承父类的属性或方法
3. 多态
   + 子类可以代替父类出现,导致一个对象可以呈现子类，父类的多种形态 

xxx 解决哪些问题？
xxx 的架构,模块

