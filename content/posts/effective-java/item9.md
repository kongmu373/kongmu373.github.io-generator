---
title: "Item9"
date: 2020-06-14T14:35:29+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 9: Prefer try-with-resources to try-finally（使用 try-with-resources 优于 try-finally）


## try-finally
+ 从历史上看，try-finally 语句是确保正确关闭资源的最佳方法
```java
// try-finally - No longer the best way to close resources!
static String firstLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```
+ 但添加第二个资源时，情况会变得更糟：
```java
// try-finally is ugly when used with more than one resource!
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);
    try {
        OutputStream out = new FileOutputStream(dst);
    try {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        while ((n = in.read(buf)) >= 0)
            out.write(buf, 0, n);
    } finally {
        out.close();
        }
    }
    finally {
        in.close();
    }
}
```
+ 使用 try-finally 语句关闭资源的正确代码（如前两个代码示例所示）也有一个细微的缺陷。try 块和 finally 块中的代码都能够抛出异常。


##  try-with-resources 
下面是使用 try-with-resources 的第一个示例：
```java
// try-with-resources - the the best way to close resources!
static String firstLineOfFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```
下面是使用 try-with-resources 的第二个示例：
```java
// try-with-resources on multiple resources - short and sweet
static void copy(String src, String dst) throws IOException {
    try (InputStream in = new FileInputStream(src);OutputStream out = new FileOutputStream(dst)) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        while ((n = in.read(buf)) >= 0)
            out.write(buf, 0, n);
    }
}
```

## 总结
+ 在使用必须关闭的资源时，总是优先使用 try-with-resources，而不是 try-finally。
+ 前者的代码更短、更清晰，生成的异常更有用。
