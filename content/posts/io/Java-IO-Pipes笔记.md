---
title: "Java IO Pipes笔记"
date: 2020-07-18T17:38:12+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io/pipes.html) 作者: Jakob Jenkov
### Creating Pipes via Java IO
+ 通过`PipedOutputStream`和`PipedInputStream`类使用Java IO创建管道
+ `PipedInputStream`应该被连接到`PipedOutputStream`
+ sample:
```java
import java.io.IOException;
import java.io.PipedInputStream;
import java.io.PipedOutputStream;

public class PipeExample {

    public static void main(String[] args) throws IOException {

        final PipedOutputStream output = new PipedOutputStream();
        final PipedInputStream  input  = new PipedInputStream(output);


        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    output.write("Hello world, pipe!".getBytes());
                } catch (IOException e) {
                }
            }
        });


        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                try {
                    int data = input.read();
                    while(data != -1){
                        System.out.print((char) data);
                        data = input.read();
                    }
                } catch (IOException e) {
                }
            }
        });

        thread1.start();
        thread2.start();

    }
}
```
+ 注意`PipeInputStream`于`PipeOutputStream`都有一个`connect()`方法连接到其他 pipe stream

## Pipes and Threads
+ 使用两个已经连接的管道流，要将两个管道流放到不同的线程中
+ 在管道流中调用`read()`或者`write()`都会阻塞
+ 如果只用一个线程那就会导致deadlocking.

## Pipe Alternatives(替代)
+ 除了管道，线程还有许多其他方法可以在同一JVM中进行通信。
+ 实际上，线程更经常交换完整的对象而不是原始字节数据。
+ 但是你需要在线程之间交换原始字节数据，则可以使用Java IO的管道。

