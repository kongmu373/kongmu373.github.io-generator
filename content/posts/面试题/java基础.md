---
title: "Java基础"
date: 2020-07-03T16:28:56+08:00
draft: true
tags: ["面试题"]
series: ["面试题"]
categories: ["面试题"]
---

## Java程序的运行原理
1. 当点击编译器的运行按钮后，发生了什么呢?(分为编译过程和启动过程)
     1. .java文件执行编译(被Compiler的编译器处理)，生成.calss文件(字节码)
     2.  .class文件被jvm识别和加载
+ 即成链状: .java -(编译)-> .class -(被识别)-> jvm
+ 编译后的代码放在 target/classes目录中 或者打包成jar包
+ 字节码保存类所有的信息,可以使用.class文件或jar包在任何机器上运行

2. 平台无关性和跨平台性怎么实现的呢?
   + 每个平台上都搭建一个jvm
   + jvm能识别字节码
   + jvm负责把统一的字节码翻译成底层的系统调用

## JDK/JRE有什么区别?()
+ JRE, Java Runtime Exception(运行环境) 如果只需要字节码运行在jvm里面，这时候只需要jre就可以了,它是能够把字节码翻译成不同系统调用的东西.
+ JDK, Java Development Kit(开发工具包) 如果还需要编译java代码就需要jdk
+  简单来说，相当于 jdk = jre + javac 

## jre与jvm的关系
+ jre是个软件名字，jvm是个抽象虚拟计算机的概念，我们通常把jre的环境称为jvm.

## Java的基础类型有哪些?
+ char/byte/short/int/long/double/float/boolean

## 除了上述的基本类型，其他都是引用类型，没有例外

## String 是基本数据类型吗？
+ 不是
+ 有个基本的判定，所有基本类型都是小写字母开头的，大写字母开头的都是引用类型.

## Java的参数传递是传值还是传引用?
  + Java世界中的一切对象都是指针(地址).
  + 函数调用都是传值，但是如果传递的是引用类型，就是传地址的拷贝
```java
// cat是引用类型，本事就是存的地址 
Cat cat = new Cat();
foo(cat, 1);

void foo(Cat cat, int i) {
    //cat 拿到的是地址 12345 ， i拿到的是1
}
```

## StringBuffer/StringBuilder的区别/线程安全性?
+ 注意:如果没有额外声明，所有的类默认都是线程不安全的
+ 首先StringBuilder更常用也更快些，但是StringBuilder是线程不安全的
+ StringBuffer稍慢些，但是线程安全

## Object有哪些常用方法?
+ equals方法 判断两个对象是否相等
  + equals的约定:  
    + 自反性
    + 对称性
    + 传递性
    + 持久性
  + 默认实现是判断两个对象是否相同(即 ==)
+ getClass方法 获取对象的类(说明书)
+ toString方法 显示该对象是长什么样的

## native
+ native标识的方法，表示该方法的实现是与平台相关的，没办法在class文件中显示出来,因此在运行的时候，jvm要在当前平台里面找到该方法的实现，该方法才能进行调用

## String有哪些常用方法?
+ 一堆构造函数, byte[], char[] ....
+ compareTo(String):int 比较大小,因此可以被TreeMap所处理
+ getBytes():byte[],getChars():char[]
+ indexOf(...):int  检查一下目标字符串在自己的第几个
+ isEmpty():boolean 判断是否为空字符串
+ length():int 判断字符串的长度
+ split(String):String[] 根据参数的分隔符分割成不同的数组
+ subString(int):String 
+ startsWith(String):boolean, endsWith(String):boolean
+ toUp/LowerCase():String
+ trim()
+ valueOf()
