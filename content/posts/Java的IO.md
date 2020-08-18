---
title: "Java的IO"
date: 2020-05-29T11:45:59+08:00
draft: true
tags: ["IO"]
series: ["爬虫"]
categories: ["Java体系"]
---
# Java的IO
> IO就是输入输出

+ 什么是IO呢？
+ InputStream/OutputStream
+ IO的使用
  
## 计算机体系原理
+ 计算机分三级 
    + CPU
      + 频率  3GHz -> 每秒30亿次（每秒运行30亿条指令 10亿换算成 1纳秒） 即1条指令大约要0.3纳秒 
    + 内存 memory
      + 存数据
      + 断电丢失
    + 硬盘 (Hard Disk -- HDD) 固态 SSD      
      + 存数据
      + 容量大，断电不丢失
      + 比内存慢

## 文件的本质
+ 一段字节流:
  + 文本文件(txt/代码)
  + 二进制文件
+ 每个程序负责解释文件中的字节流
+ 注意: 程序 <=>文件(读取文件/写入文件)，浏览器 <=> 服务器(Request/Response),都是IO(InputOutput,输入输出)
+ 输入，输出时站在应用程序角度来看的
  
## InputStream/OutputStream
+ InputStream 
  + superclass of all classes representing an input stream of bytes.
  + 关键方法 read()
+ OutputStream
  + superclass of all classes representing an ouput stream of bytes.
  + 关键方法 write(int b)
```java 
// 抽象的输入/输出操作
// 1. 文件输入/输出
// 请永远使用绝对路径
// 输入
InputStream is = new FileInputStream("...");
int firstByte = is.read();
sout(firstByte);
while(true) {
    int b = is.read();
    if(b == -1) {
        break;
    }
    sout((char)c)
}

// 输出
OutputStream os = new FileOutputStream("...");
os.write('p')

// 网络读取字节
HttpClient client = HttpClients.createDefault();
HttpGet get = new HttpGet("https://baidu.com");
HttpResponse response = client.execute(get);
InputStream is = response.getEntity().geContent();
sout(is.read()); // <
```

## Java中的File 
+ java.io.File
+ File并不代表一个文件，它只代表一个路径
+ 抽象的文件(指文件或者文件夹)
+ File的常见方法
    + isDirectory()/isFile()/exists()
    + getAbsolutePath()/listFiles()/list()/isAbsolute()/getName()
    + ...
+ 绝对路径与相对路径
  + 绝对路径能唯一确定文件/文件夹的位置
  + 相对路径相对的就是JVM进程的当前工作目录
+ 读/写文件
  + 缓存(buffer,cache)
    + BufferedReader/BufferedWriter
    + 在内存中创建好，一次写入
+ NIO (Java 7+) 
  + New IO or Non-blocking IO 非阻塞IO
  + NIO的Path -- 就是旧版本的File 
  + NIO的Files类
+ 需要任何IO的功能，尽管搜索，肯定有人把轮子造好了
  + FileUtils
  + IOUtils


