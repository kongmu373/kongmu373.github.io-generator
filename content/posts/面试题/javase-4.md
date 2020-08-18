---
title: "线程问题"
date: 2020-08-09T08:30:56+08:00
draft: true
categories: ["Java"]
---

## 线程的状态图
参考: https://www.zhihu.com/question/56494969

## 线程的概念
+ 是CPU调度的基本单位，多个线程共享同一个进程的资源
+ 同一个进程,线程之间栈是隔离的，而堆区和方法区共享
## 线程的启动方式
+ Thread
+ Runnable
+  ThreadPoolExecutor 

## 线程的常用方法
+ run
  + 继承 Thread 类 覆写的方法
+ start
  + 开启一个线程
+ join
  + 将别的线程加入到当前线程运行
+ sleep
  + 不需要销毁资源
+ yield
  + 将该线程重新加入到就绪队列
+ interrupt
  + 打断该线程，并抛出异常
+ setDaemon
  + 设置为守护线程
## 线程池解决了什么问题?
+ 节约创建线程和销毁线程的时间

## synchronized
+ 为了线程同步
+ 锁的时对象而不是代码
  + 常见如 this, XX.class
  + 不能使用String常量, Integer, Long
+ 锁定方法，非锁定方法 可以同时执行，如果访问的是同一资源可能产生脏读的问题
+ 异常也会释放锁
+ JDK早期 重量级 OS
+ 改进 锁升级
  + sync (Object)
  + markword 记录这个线程ID (偏向锁)
  + 如果有线程争用时: 升级为 自旋锁
  + 自旋10次以上，升级为重量级锁 - OS 
+ 什么情况使用自旋锁比较好呢？（如 Atomic, Lock等)
  + 一般来说，自选锁是只在用户态，而不用经过内核态，所以效率会高些
  + 如果执行时间较短，争用的线程数目小就使用自旋锁比较好
  + 但是自旋锁会空转CPU，一般执行时间长,线程数目多还是使用重量级锁(OS的锁)比较合适

## 如何理解互斥锁、条件锁、读写锁以及自旋锁？
参考:https://www.zhihu.com/question/66733477

## Object的wait,notify,notifyAll方法
+ 这三个方法只能在sychronized(即代码同步块)里面调用
+ wait，释放锁，让线程进入阻塞状态
+ 等待别的线程调用notify/notifyAll唤醒,获得锁进入就绪状态，等待CPU调度

## volatile
+ 保证线程可见性
  + CPU的MESI 缓存一致性协议
+ 禁止指令重排序(CPU)
  + DCL单例
  + Double Check Lock
```java
public class Main {
    private static volatile Object single;

    public static Object getInstance() {
        if (single == null) {
            synchronized (Main.class) {
                if (single == null) {
                    single = new Object();
                }
            }
        }
        return single;
    }

    public static void main(String[] args) {
        Object instance = getInstance();
//        new对象的指令如下, 可能发送指令重排序，即new 对象已经赋值到single上,
//        然后就去检测 `single == null` 这样就返回了一个没有初始化的对象
//        为该单例添加 volatile 可以避免这种情况
//        当然这种可能性很低
//        0: new           #2                  // class java/lang/Object
//        3: dup
//        4: invokespecial #1                  // Method java/lang/Object."<init>":()V
//        7: astore_1

    }
}
```

## 关于synchronized注意的问题
+ 尽量采用细粒度的锁，可以使线程争用时间变短，从而提高效率
+ 不能用String常量作为锁的对象
```java
	String s1 = "Hello";

	void m1() {
		synchronized(s1) {
			
		}
	}
```
+ 锁的对象不能发生改变，事实上应该设置为final
`final Object o = new Object();`

## CAS (无锁优化 自旋 乐观锁)
+ Compare And Swap
+ cas(V, Expected, NewValue)
  + if V == E
    + V = NewValue
    + otherwise try again or fail
  + CPU原语支持(一条指令)
+ ABA 问题:
  + 加多一个版本号
  + 如 A 1.0
  + B 2.0
  + A 3.0
  + cas(version)
  + atomicstampedreference
  + 如果基础类型，无所谓 - 引用类型 女朋友给你复合，中间经历了别的男人

## Unsafe
+ 直接操作内存
  + allocateMemory putXX freeMemory pageSize
+ 直接生成类实例
  + allocateInstance
+ 直接操作类或实例变量
  + objectFieldOffset
  + getInt
  + getObject
+ CAS相关操作
  + compareAndSwapObject Int Long

## ReenterLock
+ 可重入的锁
+ 使用tryLock()/tryLock(time)进行尝试锁定,方法返回一个boolean
+ 调用lockInterruptibly方法
+ 默认未非公平锁，设置true切换到公平锁(`private static ReentrantLock lock = new ReentrantLock(true); `)
+ `CountDownLatch`
```java
    private static void usingCountDownLatch() {
        Thread[] threads = new Thread[100];
        CountDownLatch latch = new CountDownLatch(threads.length);

        for (int i = 0; i < threads.length; i++) {
            threads[i] = new Thread(() -> {
                int result = 0;
                for (int j = 0; j < 10000; j++) {
                    result += j;
                }
                latch.countDown();
            });
        }

        for (int i = 0; i < threads.length; i++) {
            threads[i].start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("end latch");
    }
```
+ `CyclicBarrier`
+ 