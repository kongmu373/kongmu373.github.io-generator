---
title: "Java IO Files笔记"
date: 2020-07-18T17:06:54+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io/files.html) 作者：Jakob Jenkov
## Java IO File Classes
Java IO API包含以下与在Java中处理文件有关的类：
+ File
+ RandomAccessFile
+ FileInputStream
+ FileReader
+ FileOutputStream
+ FileWriter

### Reading Files via Java IO
+ you can use `FileInputStream` or `FileReader` to read a file 
+ 这两个类：要么一个字节一个字节读，或者一个数组去读
+ 你需要按照文件的顺序去读
+ 如果你需要跳读，你可以使用`RandomAccessFile`

### Writing Files via Java IO
+ you can use a `FileOutputStream` or a `FileWriter` to write a file
+ 跟读一样，你可以选择一个字节或一个数组的方式去写文件
+ 数据按写入顺序顺序存储在文件中。
+ 如果你需要跳写，你可以使用`RandomAccessFile`

### Random Access to Files via Java IO
+ 您可以通过RandomAccessFile类使用Java IO随机访问文件。

### File and Directory Info Access
+ 如果你想要获取文件的信息,你可以通过`File`类,获取如:
  + 文件大小
  + 文件属性
  + 获取文件夹中所有的文件 
  + ... 等等

