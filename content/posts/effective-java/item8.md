---
title: "Item8"
date: 2020-06-13T10:43:42+08:00s
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 8: Avoid finalizers and cleaners（避免使用终结器和清除器）

## 终结器和清除器的缺点
+ 缺点:
    + 不能保证它们会被立即执行(不仅不能保证终结器或清洁剂能及时运行；它并不能保证它们能运行。)
    + 在终结期间抛出的未捕获异常被忽略，该对象的终结终止。
    + 严重影响性能。使用 try-with-resources 比终结器快得多(50倍左右,清除器比终结器快点)
    + 存在严重安全问题：它们会让你的类受到终结器攻击。


## 用途
1. 作安全网(as a safety net)。一些 Java 库类，如 FileInputStream、FileOutputStream、ThreadPoolExecutor 和 java.sql.Connection，都有终结器作为安全网。
清除器作安全网的例子:
```java
// An autocloseable class using a cleaner as a safety net (Page 32)
public class Room implements AutoCloseable {
    private static final Cleaner cleaner = Cleaner.create();

    // Resource that requires cleaning. Must not refer to Room!
    private static class State implements Runnable {
        int numJunkPiles; // Number of junk piles in this room

        State(int numJunkPiles) {
            this.numJunkPiles = numJunkPiles;
        }

        // Invoked by close method or cleaner
        @Override public void run() {
            System.out.println("Cleaning room");
            numJunkPiles = 0;
        }
    }

    // The state of this room, shared with our cleanable
    private final State state;

    // Our cleanable. Cleans the room when it’s eligible for gc
    private final Cleaner.Cleanable cleanable;

    public Room(int numJunkPiles) {
        state = new State(numJunkPiles);
        cleanable = cleaner.register(this, state);
    }

    @Override public void close() {
        cleanable.clean();
    }
}
```

+ 好的实现方式
```java
// Well-behaved client of resource with cleaner safety-net (Page 33)
public class Adult {
    public static void main(String[] args) {
        try (Room myRoom = new Room(7)) {
            System.out.println("Goodbye");
        }
    }
}
```

+ 不好的实现方式
```java
// Ill-behaved client of resource with cleaner safety-net (Page 33)
public class Teenager {
    public static void main(String[] args) {
        new Room(99);
        System.out.println("Peace out");

        // Uncomment next line and retest behavior, but note that you MUST NOT depend on this behavior!
        System.gc();
    }
}
```

## 总结
+ (Finalizers and Cleaners)终结器和清除器不是析构函数
+ 不能保证终结器和清除器会立马执行，甚至不能保证他们一定会执行
+ `System.gc` 只是一个hint,不是gc的调用
+ 终结器和清除器的使用会导致严重的性能损失
+ 使用显式的方法(methods)去终结像`close()`(现在最好使用try-with-resources块)