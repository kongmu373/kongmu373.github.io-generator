---
title: "JavaIONetworking笔记"
date: 2020-07-20T13:46:28+08:00
draft: true
categories: ["IO"]
---

[原文链接](http://tutorials.jenkov.com/java-io/networking.html) 作者: Jakob Jenkov

### Networking
+ 一旦为两个进程建立网络连接，它们之间的网络通信就如同文件处理一样:
  + 使用`inputStream`读取数据
  + 使用`ouputStream`写入数据
+ 即可以将文件处理的操作套用在网络连接的处理上(毕竟FileOutputStream is a subclass of OutputStream)

### sample
+ 在下面的例子中process方法接受的是Inputstream的参数,无论来自文件的IO还是网络IO.
```java
public class MyClass {
    

    public static void main(String[] args) {

        InputStream inputStream = new FileInputStream("c:\\myfile.txt");
    
        process(inputStream);
    
    }
    

    public static void process(InputStream input) throws IOException {
        //do something with the InputStream
    }
}
```
