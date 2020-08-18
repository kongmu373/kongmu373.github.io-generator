---
title: "34 Serializable"
date: 2020-07-23T15:33:01+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io) 作者：Jakob Jenkov

+ Java Serializable接口(java.io.Serializable是类要进行序列化和反序列化时必须实现的标记接口)
+ Java对象序列化（写入）通过ObjectOutputStream完成，反序列化（读取）通过ObjectInputStream完成
+ Serializable是一个标记接口，意味着它不包含任何方法
  + 因此,实现Serializable的类不必实现任何特定的方法

## Serializable Example
```java
import java.io.Serializable;

public static class Person implements Serializable {
    public String name = null;
    public int    age  =   0;
}
```
+ 如上所述,Person类实现了Serializable接口，但实际上并未实现任何方法


## serialVersionUID
+ 如果 implement Serializable接口, 那么这个可序列化类应该包含一个private static final long 变量 命名为 serialVersionUID.
+ 用来在反序列化的时候来鉴别该对象是不是与序列化时同一个版本的类
+ 一般来说, serialVersionUID不需要手写，Java SDK和许多Java IDE包含用于生成serialVersionUID的工具

```java
import java.io.Serializable;

public static class Person implements Serializable {

    private static final long serialVersionUID = 1234L;

    public String name = null;
    public int    age  =   0;
}
```

## Object Serialization Today
+ 许多Java项目使用与Java序列化机制不同的机制来序列化Java对象
+ 如，Java对象被序列化为JSON，BSON或其他更优化的二进制格式，在Web浏览器中运行的JavaScript可以在本地对JSON进行序列化和反序列化
+ 顺便说一下，这些其他对象序列化机制通常不需要Java类来实现Serializable
+ 通常使用Java Reflection检查类，因此实现Serializable接口是多余的

## More Information About Serialization
oracle参考资料:http://www.oracle.com/technetwork/articles/java/javaserial-1536170.html
