---
title: "多线程基本原理"
date: 2020-06-01T10:41:18+08:00
draft: true
tags: ["多线程"]
series: ["多线程"]
categories: ["Java体系"]
---

+ 为什么需要多线程?
+ Thread线程
+ Java中线程的表示
+ 多线程问题的来源
+ 多线程的应用场景

## 一、为什么需要多线程?
1. Java的执行模型是同步/阻塞的
  + 如果你执行非常耗时的操作，当前方法的执行流会阻塞，等待耗时的操作执行完
```java
// main方法要打印"main"，要等待a方法执行完，a方法等待b方法完，由于执行非常耗时，期间形成阻塞.
public static void main(String[] args) throws InterruptedException {
    a();
    System.out.println("main");
}

private static void a() throws InterruptedException {
    Thread.sleep(10000);
    b();
}

private static void b() throws InterruptedException {
    Thread.sleep(5000);
}
```
2. [CPU: 你们都慢! 死! 了](https://cizixs.com/2017/01/03/how-slow-is-disk-and-network/)
  + CPU做一条指令 0.3ns 
  + 内存 10us
  + 磁盘 更慢
  + cpu>>>>内存>>>>磁盘 (形成阻塞)
  + 尽可能利用CPU,就需要多线程
3. 现代CPU都是多核的
4. 默认情况下只有一个线程
  + 一个线程处理问题是非常自然的
  + 但是具有严重的性能问题

## 二、Thread线程
```java
// 能不能多情几个人来干活?不用阻塞呢?
    public static void main(String[] args) throws InterruptedException {
        slowOperation1();
        slowOperation2();
        slowOperation3();
        slowOperation4();
        ...
    }
```
> 一个线程就是一个工人,为每一个很慢的操作去雇佣一个新的工人

```java
// 能不能多情几个人来干活?不用阻塞呢?
    public static void main(String[] args) throws InterruptedException {
        // Thread(Runnable runnable)
        // new Thread(new Runnable() {
        //     @Override
        //     public void run () {
        //         slowOperation1();
        //     }
        // })
        // 声明线程,并start
        new Thread(Main::slowPeration2).start();
        new Thread(Main::slowPeration3).start();
        new Thread(Main::slowPeration4).start();
        slowOperation1();
        // slowOperation2();
        // slowOperation3();
        // slowOperation4();
        ...
    }
```
+ 每当开启一个新线程，就多了一个方法栈，方法栈能像主线程一样执行方法的调用.(以run为起点)
![多线程](/img/多线程.jpg)
<center style="font-size:14px;color:#C0C0C0;text-decoration:underline">多线程的方法栈</center>

+ Thread 
  + 当主线程一条语句开启一个新线程的时候，主线程不需要等待它运行结束，可以立即返回，执行下一条语句
  + Java中只有这么一种东西代表线程
  + start方法才能并发执行
    + 不能使用run方法(使用该方法变成同步了)
  + 每多开启一个线程，就多一个执行流(多了一个工人)
  + 方法栈(局部变量)是线程私有的
  ![线程私有的](/img/线程私有的.jpg)
 <center style="font-size:14px;color:#C0C0C0;text-decoration:underline">方法栈(局部变量)是线程私有的</center> 

  + 静态变量/类变量是被所有线程共享的(多线程的坑)

## 三、多线程问题的来源
> 多线程问题就想象成多个人看着同一份代码，以疯狂地乱序执行它。

+ 在多线程的某一个时刻，并不知道哪个线程在执行。
![process](/img/process.png)
<center style="font-size:14px;color:#C0C0C0;text-decoration:underline">一个进程（process）含有两个线程（threads）的运行</center>

+ 宏观上，是多个工人完成任务(像是并行的)，微观上，是并发的。
+ 共享变量的坑
```java
// 最后输出的值可能小于等于1000，不确定
public class Main {
    private static int i = 0;
    public static void main(String[] args) throws InterruptedException {
        for (int j = 0; j < 1000; j++) {
            new Thread(Main::modifySharedVariable).start();
        }
    }
    private static void modifySharedVariable() {
        // 不是原子操作
        // 分三步
        // 1. 取i的值
        // 2. 把i的值加1
        // 3. 把修改的值写回i
        i++;
        System.out.println("i: " + i);
    }
}
```
  + 原因如图：
  ![微观多线程的运行](/img/i++.png)
  <center style="font-size:14px;color:#C0C0C0;text-decoration:underline">i++中i变量可能小于1000的原因</center>


  ## 多线程的应用场景
  + 对于IO密集型的应用极其有用
    + 网络IO (通常包括数据库)
    + 文件IO
