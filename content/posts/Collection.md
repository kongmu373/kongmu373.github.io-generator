---
title: "Collection"
date: 2020-05-27T10:55:23+08:00
draft: true
tags: ["Collection"]
series: ["Collection"]
categories: ["Java体系"]
---

## Collection 简介
1. Collection是什么？(root interface)
  + 是接口，不是类，不能被new
  + 一组东西，一篮子鸡蛋,只包含一种元素
2. Collection 体系 (root interface)
    + 例如,ArrayList的Diagrams
    ![Digrams](/img/ArrayList.jpg)

3. Collection 常使用的方法:
  + new: new ArrayList(Collection), new ArrayList()

    ```java
    Collection<Integer> c = new LinkedHashSet();
    // IntegerList
    List<Integer> list = new ArrayList<>(c);

    // == 
    List<Integer> list2 = new ArrayList<>();
    list2.addAll();

    // == 
    List<Integer> list3 = new ArrayList<>();
    for(Integer i : c) {
        list3.add(i);
    }
    ```
  + R: size()/isEmpty()/contains()/for()/stream()
  + C/U: add()/addAll()/retainAll()
    ```java
    Collection<Integer> c = new LinkedHashSet();
    c.add(1);
    c.add(2);

    List<Integer> list = new ArrayList<>();
    list.add(2);
    list.add(3);

    list.retainAll(c); //只剩 2了
    ```
  + D: clear()/remove()/removeAll()

## List
1. List是什么？
   + 有序的，同一元素的集合。 
2. List中，最常用的实现类ArrayList
  + 本质上就是一个数组
   + 面试题: 动态扩容
     + 当原本的数组空间满了，就创建一个更大的空间（1.5倍），然后把原先的所有元素拷贝进去
     + add()方法，调用该方法时，如果数组空间满了，ArrayList自动扩容
  
## Set
1. Set是什么?
   + 不允许有重复元素的集合
2. 如何判断是否有重复元素呢？
   + 通过 equals方法 判断重复元素
3. 但是仅仅通过equals方法判断，效率太低了。
```java
// 自己实现的，效率低的例子
class MySet {
    private List<Integer> myData = new ArrayList<>();
    public void add(Integer data) {
        if(!myData.contains(data)){
            myData.add(data);
        }
    } 
}
``` 
4. 所以需要hashCode
   1. Java世界里第二重要的约定:hashCode(第一重要约定为:equals)
   2. hashCode要遵守的约定:
      + 同一个对象必须返回相同的hashCode 
      + 两个对象的equals为true,必须返回相同的hashCode
      + 两个对象不同，也可能返回相同的hashCode


## 哈希算法
1. 哈希是什么?
    + 哈希就是一个单向的映射
2. 哈希的例子:
   1. 从姓名到姓的映射
   2. 字典
   3. 从任意对象到一个整数的hashCode

## HashSet
1. HashSet是什么？
    + 是Set 最常用，最高效的实现类
2. HashSet是如何使用以及为什么使用?
    + 实战: HashSet的高效性(同样是查找元素，比ArrayList要快得多，使用了哈希桶)
    + 实战: 使用HashSet对ArrayList去重
```java
// 去重
List<Integer> list =new ArrayList<Integer>();
list.add(1);
list.add(2);
list.add(2);
list.add(3);
Set<Integer> set = new HashSet<>(list);// 1,2,3
```
3. HashSet特性
   + 无序的，(有序的，可以选择LinkedHashSet) 


## Map
1. Map是什么？
 + 是一种键，值映射表
 + 每一个键，值对叫做Entry。
2. Map的常用方法:
+ C/U: put()/putAll()
    + put方法的原理:
      + 插入一个元素，利用哈希函数确定该Entry的插入位置(index)
      + 因为Map对应的hash数组是有限的，难免会出现index冲突，所以需要链表来解决(java1.7以后还引入了红黑树),链表采用头插法(因为作者认为后来的元素被搜索的几率更高)
+ R: 
  + get() / size()
    + 先把输入的值做一次哈希映射，得到对应的index,然后到对应的hash数组查找对应的index,如果第一个不是，再顺着对应的链表或红黑树去找.
  + containsKey() / containsValue()
  + keySet() / values() / entrySet()
    + 有个坑 
    + 下面是Map.java的keyset()上的注释，意思是无论改变map，还是keySet()(values/entrySet也是一样),两者都会改变.
    ```java
    
    /**
     * Returns a {@link Set} view of the keys contained in this map.
     * The set is backed by the map, so changes to the map are
     * reflected in the set, and vice-versa.
     * ....
     * /
  
    ```
+ D: remove()/clear()
  
## HashMap
1. HashMap是什么?
  + 每一对Entry分散存储在一个数组当中，这个数组就是HashMap的主干
  + 最常用，最高效的map的实现类
  + HashSet是由HashMap实现的，两者本质是一种东西
2. HashMap的常见面试题
   1. HashMap的扩容 Resize
      + 影响扩容的因素有两个:
        1. Capacity(HashMap当前的长度 2的幂)
        2. LoadFactor(HashMap负载因子，默认值为0.75f)
      + 衡量是否需要扩容:
        `HashMap.Size >= Capacity * LoadFactor`
      + Resize步骤:
        1. 扩容(创建一个新的Entry空数组，长度是原来的2倍)
        2. ReHash 
```java
void transfer(Entry[] newTable, boolean rehash) {
    int newCapacity = newTable.length;
    for (Entry<K,V> e : table) {
        while(null != e) {
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        }
    }
}
```
   1. HashMap的线程不安全(一般多线程无脑使用C)
      + Hashmap的Resize包含扩容和ReHash两个步骤，ReHash在并发的情况下可能会形成链表环，导致死循环
      + 一般多线程使用ConCurrentHashMap 
   2. HashMap的初始长度是16，并且每次扩容都是2的幂(之所以16是为了服务于从key映射到index的Hash算法)
      1. index = HashCode(key) & (Length - 1)
      2. 必须保证HashMap的长度是2的幂，保证index值均匀
   3. HashMap在Java 7+后的改变 链表->红黑树

HashMap的两篇博客:[什么是HashMap？](https://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653191907&idx=1&sn=876860c5a9a6710ead5dd8de37403ffc&chksm=8c990c39bbee852f71c9dfc587fd70d10b0eab1cca17123c0a68bf1e16d46d71717712b91509&scene=21#wechat_redirect)
[高并发下的HashMap](https://juejin.im/post/5a224e1551882535c56cb940)

## 有序集合 TreeSet/TreeMap
+ TreeSet(排序的Set)
  + 与其他Set进行比较
  ```java
          List<Integer> list = Arrays.asList(101, -1, 100, 3, 2, 1, 5, 4);
        Set set1 = new HashSet(list);
        Set set2 = new LinkedHashSet(list);
        Set set3 = new TreeSet(list);
        set1.forEach(item->System.out.printf("%d,",item));
        System.out.println();
        set2.forEach(item->System.out.printf("%d,",item));
        System.out.println();
        set3.forEach(item->System.out.printf("%d,",item));
    /* result:
    -1,1,2,3,100,4,101,5, 无序 
    101,-1,100,3,2,1,5,4, 按加入顺序 
    -1,1,2,3,4,5,100,101, 按默认从小到大排序 
    */
  ```
  + 同样TreeMap也是排序的
  + 可以指定Comparator
+ TreeSet/TreeMap 内部就是一种红黑树(自平衡二叉树)
    + 对于一个根节点来说，左边的节点都比它小，右边的节点都比它大
    + 查找的复杂度是O(logn) -- 从一般的线性时间下降到对数时间
    + 插入一个元素，对红黑树的性质不变(即左边的节点都比根节点小，右边的节点都比根节点大)



## Collections工具方法集合
+ emptySet(): 返回一个方便空集合
+ synchronizedCollection: 将一个集合变成线程安全的
+ unmodifiableCollection: 将一个集合变成不可变的(也可以使用Guava的Immutable)

## Collection的其他实现
+ Queue/Deque
  + Queue(LILO -- Last in Last out) 常用方法
    + add
    + peek
    + ...
  + Deque (支持在两端插入和删除 double ended queue)常用方法:
    + addFirst() / getFirst()
    + addLast() / getLast()
+ ~~Vector/Stack~~ 
  + 推荐使用ArrayList/Deque 去代替
+ LinkedList(链表)
+ ConcurrentHashMap(线程安全)
+ PriorityQueue(优先级队列，例子:闹钟)

## Guava(番石榴)
+ 不要重复造轮子！尽量使用经过实战检验的类库
+ Lists/Sets/Maps
+ ImmuatableMap/ImmuatableSet
+ Multiset/Multimap
+ BiMap
