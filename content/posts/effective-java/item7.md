---
title: "Item7"
date: 2020-06-12T19:30:16+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item7: Eliminate obsolete object references （排除过时的对象引用）

## 自实现的栈可能存在忽略的内存泄露
```java
mport java.util.Arrays;
import java.util.EmptyStackException;

// Can you spot the "memory leak"?
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if (size == 0)
            throw new EmptyStackException();
        return elements[--size];
    }

    /**
     * Ensure space for at least one more element, roughly
     * doubling the capacity each time the array needs to grow.
     */
    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }
}
```
+ 如果堆栈增长，然后收缩，那么从堆栈中弹出的对象将不会被垃圾收集，这是因为栈保留了这些对象的旧引用
+ 在本例中，元素数组的「活动部分」之外的任何引用都已过时。活动部分由索引小于大小的元素组成。
+ 正确版本:
```java
public Object pop() {
    if (size == 0)
        throw new EmptyStackException();
    Object result = elements[--size];
    elements[size] = null; // Eliminate obsolete reference
    return result;
}
```

## 缓存的内存泄露
+ 一旦将对象引用放入缓存中，就很容易忘记它就在那里，并且在它变得无关紧要之后很久仍将它留在缓存中
+ 解决方案:
  1. 只要缓存外有对其键的引用，那么就将缓存表示为 WeakHashMap
  2. 随着时间的推移，条目的价值会越来越低。在这种情况下，缓存偶尔应该清理那些已经停用的条目。这可以通过后台线程（可能是 ScheduledThreadPoolExecutor）或向缓存添加新条目时顺便完成。 

## 侦听器和其他回调的内存泄露
 如果你实现了一个 API，其中客户端注册回调，但不显式取消它们，除非你采取一些行动，否则它们将累积。确保回调被及时地垃圾收集的一种方法是仅存储对它们的弱引用，例如，将它们作为键存储在 WeakHashMap 中。
 

## 总结
+ 自实现的栈，活动部分之外的任何引用都已过时，造成内存泄露
+ 缓存中，过时的，无关紧要的对象放在缓存中，造成内存泄露
+ 侦听器和其他回调的内存泄露
+ 解决方案：第一种内存泄露将不在活动部分的元素置为null,
    第二，三种泄露情况使用弱引用的WeakHashMap存储
    
+ 关于WeakHashMap的使用参考这个[文章](https://juejin.im/entry/5a085809f265da430c114c8b)