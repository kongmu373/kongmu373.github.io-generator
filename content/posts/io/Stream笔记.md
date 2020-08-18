---
title: "Java IO Stream笔记"
date: 2020-07-18T17:18:03+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io/streams.html) 作者: Jakob Jenkov
## InputStream 
+ 类java.io.InputStream是所有Java IO输入流的基类
+ 如果你写的组件需要从流中读数据，最好让你的组件依赖于`InputStream`,而不是它的子类.
+ sample:
```java
InputStream input = new FileInputStream("xxxx");
int data = input.read(); // return a int containing the byte value of the byte read.

// If there is no more data to be read, the read() method typically returns -1;
while(data != -1) {
    data = input.read();
}
```

## OutputStream
+ 类java.io.OutputStream是所有Java IO输出流的基类
+ 如果你写的组件需要写数据到流中，最好让你的组件依赖于`OutputStream`,而不是它的子类.
+ sample:
```java
OutputStream output = new FileOutputStream("c:\\data\\output-file.txt");
output.write("Hello World".getBytes());
output.close();
```

## Combining Streams
+ 您可以将流组合成链，以实现更高级的输入和输出操作。
+ sample:  wrap your `InputStream` in an `BufferedInputStream`
```java
// It is faster to read a larger block of data from the disk and then iterate through that block byte for byte afterwards.
InputStream input = new BufferedInputStream(
                        new FileInputStream("c:\\data\\input-file.txt"));
...
```
+ Buffer同样作用于`Output`
+ Buffering只是合成流中的一种选择，可以选择其他流，甚至可以选择同类的流或者自定义流

