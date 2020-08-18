---
title: "Java IO Overview笔记"
date: 2020-07-18T16:31:40+08:00
draft: true

categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io/overview.html) 作者: Jakob Jenkov

## 输入与输出 - Source and Destination
+ Java IO package 主要用于从`source`读取`raw data`,或者写一些`raw data`到一个`destination`.
+ 其中`source`与 `destination` 主要有以下这些:
  + Files
  + Pipes
  + Network Connections
  + In-memory Buffers(e.g. arrays)
  + System.in,out,error
![source and destination relation](/img/source-destination.jpg)

## stream 流
+ stream是连续无限的数据流
+ 能够从stream中读，或写数据
+ 一个stream是连接于`data source` 或者 `data destination`.
+ Java IO 可以基于字节被读写，或者基于字符被读写

### InputStream, OutputStream, Reader and Writer
+ 如果你需要从`source`读数据,那么就需要一个`InputStream`(or `Reader`).
+ 如果你需要向`destination`写数据，那么就需要一个`OutputStream`(or `Writer`)
![This is also illustrated in the diagram](/img/inputstream-outputstream.jpg)

### Java IO的目的和功能
+ Java IO 有许多子类，为什么呢?
  + 这些子类都在解决各种不同的目的,如:
    + File Access
    + Network Access
    + Internal Memory Buffer Access
    + Inter-Thread Communitaion(Pipes)
    + Buffering
    + Filtering
    + Parsing
    + Reading and Writing Text(Readers/Writers)
    + Reading and Writing Primitive Data
    + Reading and Writing Objects

### Java IO类概述表
![IO](/img/IO.png)
 	 	 
