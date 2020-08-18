---
title: "15 JavaIO: FileInputStream"
date: 2020-07-20T17:47:35+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io) 作者：Jakob Jenkov

----
## Java FileInputStream
+ Java FileInputStream类java.io.FileInputStream使得可以将文件的内容读取为字节流
+ Java FileInputStream类是Java InputStream的子类
----

### Java FileInputStream Example
```java
InputStream input = new FileInputStream("c:\\data\\input-text.txt");

int data = input.read();
while(data != -1) {
  //do something with data...
  doSomethingWithData(data);

  data = input.read();
}
input.close();
```

### FileInputStream Constructors
+ FileInputStream主要有3个构造器，这里提及其中的两个:
   1. 构造函数将String作为参数,该String包含文件系统中要读取的文件所在的路径
    ```java
    // On unix the file path could have looked like this:
    // String path = "/home/jakobjenkov/data/thefile.txt";
    //windows
    String path = "C:\\user\\data\\thefile.txt";
    FileInputStream fileInputStream = new FileInputStream(path);
    ```
   2. ileInputStream构造函数将File对象作为参数,File对象必须指向您要读取的文件.
    ```java
    String path = "C:\\user\\data\\thefile.txt";
    File   file = new File(path);

    FileInputStream fileInputStream = new FileInputStream(file);
    ```

### read()
+ 返回一个包含读取数据的字节int值
+ 如果返回的是-1，则表示没有更多的数据可以读取，可以关闭它了
+ sample:
```java
FileInputStream fileInputStream = 
    new FileInputStream("a string.");
int data =fileInputStream.read();
while(data != -1) {
    // do something with data variable

    data = fileInputStream.read(); // read next byte
}
```

### read(byte[])
+ 作为InputStream的FileInputStream还具有两个read（）方法，可以将数据读入字节数组
+ 顺便说一下，这些方法是从Java InputStream类继承的
+ 包括:
  + int read(byte[])
  + int read(byte[], int offset, int length)
+ sample:
```java
FileInputStream fileInputStream = 
    new FileInputStream("file path for string.");
byte[] data = new byte[1024];
int byteRead = fileInputStream.read(data,0,data.length);
while(byteRead != -1) {
  doSomethingWithData(data, bytesRead);

  bytesRead = fileInputStream.read(data, 0, data.length);
}
```

+ 注意:read(data, 0, data.length) 等价于 read(data)

### Read Performance
+ 一次读取一个字节数组要比一次从Java FileInputStream读取单个字节快
+ ...

### Transparent Buffering via BufferedInputStream
+ 可以使用Java BufferedInputStream从FileInputStream添加透明的自动读取和缓冲字节数组
+ BufferedInputStream从FileInputStream读取字节块到字节数组
+ 可以从BufferedInputStream逐个读取字节，并且仍然可以通过读取字节数组而不是一次读取一个字节来获得很多加速
+ sample:
```java
InputStream input = new BufferedInputStream(
    new FileInputStream("file path."),
    1024 * 1024 /* buffer size */
)
```

### Close a FileInputStream
+ 手动调用close()方法关闭
+ sample:
```java
FileInputStream fileInputStream = new FileInputStream("c:\\data\\input-text.txt");

int data = fileInputStream.read();
while(data != -1) {
  data = fileInputStream.read();
}
fileInputStream.close();
```
+ 用try-with-resources去自动关闭
```java
try( FileInputStream fileInputStream = new FileInputStream("file.txt") ) {

    int data = fileInputStream.read();
    while(data != -1){
        data = fileInputStream.read();
    }
}
```

### Convert FileInputStream to Reader
+ Java FileInputStream是基于字节的数据流
+ Java IO API还具有一个基于字符的输入流`Reader`.
+ 可以使用Java InputStreamReader将Java FileInputStream转换为Java Reader
+ sample:
```java
InputStream inputStream       = new FileInputStream("c:\\data\\input.txt");
Reader      inputStreamReader = new InputStreamReader(inputStream);
```