---
title: "Javase 2"
date: 2020-08-05T13:35:00+08:00
draft: true
categories: ["Java"]
---

## 方法区
1. 类变量
2. 类信息
3. 方法信息
4. 常量池(符号引用，以表的形式存在)
   ![常量池表](https://img-blog.csdn.net/20180717162537751?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2NTk3NDUw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
面试题: `new String("hello");` 这个语句执行创建了几个对象?
如果 "hello" 这个字面值在前面已经出现过，那么只创建了一个对象,如果没有出现过那就创建了两个对象


## Java有几种数据类型?
1. 整型
   + byte 1
   + short 2
   + int 4
   + long 8 
   + char 2
2. 浮点型
   1. float
   2. double
3. 特殊型
   1. boolean
   2. 引用类型(return address)


## Java中方法的重载
+ 方法名相同，方法参数不同的方法
+ 为什么需要重载呢？
  + 因为同一个方法中无法完成多个只是类型不一样但是操作一样的动作，那么我们可以通过重载来做


## 集合常用的基本方法
+ size():int
+ isEmpty():boolean
+ contains(Object):boolean
+ toArray():Object[]
+ add(E):boolean
+ remove(Object):boolean
+ containsAll(Collection<?>):boolean
+ addAll(Collection<? extends E>):boolean
+ removeAll
+ removeIf
+ clear
+ stream
+ parllelStream

## 为什么Java8要引入在接口中有默认实现的特性呢?
+ 便于扩展接口中的方法而不破坏原有的继承体系
+ 如果没有这个特性
+ 如果我向给有很多子类的接口扩展这么一个新的功能
+ 就很难了，也不遵守开闭原则(对修改封闭对扩展开放)


## 为什么要提前为ArrayList赋值一个初始容量呢?
+ 默认初始容量为10
```java
private void grow(int minCapacity) {
    int oldCapacity = elementData.length;
    // 扩大到1.5倍
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0) {
        newCapacity = minCapacity;
    }
    if(newCapacity - Max_Array_SIZE > 0) {
        newCapacity = hugeCapacity(minCapacity);
    }
    // 复制数组
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```
避免在add操作时销毁CPU，因为，在数组进行赋值的时候消耗时非常大的


## ArrayList与LinkedList的区别?
+ ArrayList
  + 底层用数组实现
  + 它的增长因子是0.5
  + 默认初始值为10
  + 查找修改是O(1)时间复杂度
  + 增删如果不是在尾部的话，时间是复杂度是O(n)
  + 需要挪位置
    + System.arrayCopy(),将数组依次往前或往后移
+ LinkedList
  + 底层是链表实现
  + 链表查找慢，根据引用挨个查找
  + 删除跟新增比较快，因为不需要移位置，只需断开链，再连接上就好


## 线性表
+ 数据结构之线性表 (特点:逻辑上来说,数据连续放置，也就是有序的)
  + 顺序表 (ArrayList)
  + 链表 (LinkedList)
+ AbstractSequentialList(线性访问)
+ AbstractList(随机访问)

## 进程与线程的区别
+ 一个进程包括多个线程
+ 进程是操作系统分配资源的基本单位
  + 资源指的是内存等等
+ 线程是CPU调度的基本单位

## 缓存作用
+ 解决组件间速度不匹配做出的手段

## 数据结构与算法是什么?
+ 数据结构:
  + 是数据存放的一种方式
  + 用来组织数据，使得磁盘或者内存空间更有效率的存放数据
  + 提高空间的利用率和数据的查找速度
+ 算法:
  + 如何区完成一件事情
  + 好的算法指的是节约时间，节约成本地去完成

## Java异常体系
+ Throwable
  + exception
    + 检查型异常
    + 非检查型异常
  + error
    + JVM的错,如 JVM outOfmemoryerror,stackoverflowerror(栈溢出，例如不断调用方法自身)

## Java中的类的定义方式有哪几种?
+ 普通定义
+ 静态内部类
+ 普通内部类
+ 匿名类
+ 静态： 可以通过类名.xx调用

## 内部类的好处
 可以贡献所在类的内部变量，而不像访问普通类一样需要把共享的参数传递过去

## 静态内部类是如何访问到外部类的类变量?
 在编译阶段产生一个访问外部类的一个static的access访问方法,然后在静态内部类中通过外部类的类名调用
 
## 非静态内部类如何访问外部类的变量?
1. 在构造内部类的对象时,默认把外部类的对象，在调用内部类的构造器传入,这样就可以访问外部类的非船员变量
2. 对于内部类访问外部类的属性(外部类生成一个access$x00的方法来让内部类调用)
   1. 静态变量 (aload_0)
   2. 实例变量 (不需要外部类的对象)


## 泛型
+ 什么是泛型
  + 泛型就是定义一种模板，例如ArrayList<T>，然后在代码中为用到的类创建对应的ArrayList<类型>
  + 把类型明确的工作推迟到创建对象或调用方法的时候才去明确的特殊的类型
+ 为什么需要泛型
  + 早期Java是使用Object来代表任意类型的，但是向下转型有强转的问题，这样程序就不太安全
  + 有了泛型以后
    + 代码更加简洁【不用强制转换】
    + 程序更加健壮【只要编译时期没有警告，那么运行时期就不会出现ClassCastException异常】
    + 可读性和稳定性【在编写集合的时候，就限定了类型】
+ 怎么用泛型
  + 泛型类 `public class ObjectTool<T>`
  + 泛型方法 `public <T> void show(T t)`
  + 泛型派生子类 `public class InterImpl implements Inter<String>`
  + 类型通配符 如: `public void test(List<?> list)`
    + 设定通配符上限 `List<? extends Number>`
    + 设定通配符下限 `<? super Type>`
参考: https://www.zhihu.com/question/272185241/answer/366129174
+ 泛型擦除
  + 泛型是提供给javac编译器使用的
  + 生成的class文件中将不再带有泛形信息
  + 这个过程称之为“擦除”