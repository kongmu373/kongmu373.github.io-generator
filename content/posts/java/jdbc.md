---
title: "原生jdbc"
date: 2020-08-09T20:13:10+08:00
draft: true
tags: ["jdbc"]
series: ["Java体系"]
categories: ["Java体系"]
---


## JDBC的基本代码
```java

/**
 * JDBC: 这是用来干什么的呢？
 * 使用jdbc提供一个一对多的关系
 * 数据库: SQL SERVER,ORACLE,MYSQL,DB2,MARIDB,DERBY ...
 * JDBC提供了一系列的接口,让数据库去按照这个接口实现
 * 设计思路:
 * Connection接口: 提供连接数据库的操作
 * 根据OOP的原则: 接口隔离
 * Statement, PrepareStatement, ResultSet接口
 */

public class Demo {


    public static void main(String[] args) throws ClassNotFoundException, SQLException {
/*       1. 反射是什么?
         在运行时获取这个类信息的一种技术
         可以动态加载类 (在运行一个程序的时候，允许动态的加载类进入)
         2. 反射机制的核心点在哪?
          - 自然而然想到,一个对象在类加载进入JVM并且经过加载,验证，准备，解析
          - 初始化<clinit>后的Class对象 (这个对象我讲过代表了一个类本身) 那么拿到这个对象 我就能获取到类的所有信息
         3. 怎么拿? 对象.getClass 类名.class
         4. Java的类加载机制
            怎样保证加载的类的安全性?
            不可能让危害计算机的代码，也就是类进入JVM 导致用户信息泄露或者信息被删除... 等恶意操作行为的发生
            java引入了双亲委派机制和安全沙箱保证JVM的安全
            双亲委派:   BootstrapClassloader(C语言写的，在java里面拿到的只能是null), ExtClassloader, AppClassloader,用户自定义的Classloader
         */


//        new Driver(); //也可以哦
        Class.forName("com.mysql.cj.jdbc.Driver");  // 加载一个类,放在方法区里面
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/test", "root", "123");
        System.out.println(connection);
        /**
         * ORM 框架是什么？
         * 对象关系映射，也就是把本来在数据库中以表的形式存储的数据，映射到Java对象上
         * 表: 行和列 => 对象: 属性
         * 行 => 对象
         * 列 => 对象.属性
         
         * 设计ORM的步骤如何设计?
         * 由数据库 -> 对象的过程
         * 1. 找到数据库
         * 2. 找到表
         * 3. 读取表中相应行,列的信息
         * 4. 根据列的信息构造对象的属性
         *
         * 由对象 -> 数据库的过程
         * 1. 创建数据库
         * 2. 创建表(表名与类名相同)
         * 3. 将对象中的属性映射到表中的列
         */
    }
}
```
