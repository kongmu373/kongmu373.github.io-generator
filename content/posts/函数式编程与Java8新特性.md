---
title: "函数式编程与Java8新特性"
date: 2020-05-26T08:45:15+08:00
draft: true
tags: ["Java8"]
series: ["Java8"]
categories: ["Java8"]
---

函数 就是 参数 映射成返回值.

## 函数式编程
+ "函数式编程" 是一种 "编程范式"
+ 主要思想: 把运算过程尽量写成一系列嵌套的函数调用 -> y= f(g(x)) <=> y= g(x).f().
+ 惰性计算: 并不立刻算，只有在需要产生表达式的值才开始进行运算
+ 函数是 "第一等公民": 函数与其他数据类型一样, 可以赋值给其他变量，也可以作为参数, 传入另一个函数，或者作为别的函数的返回值.
+ 纯函数 - 没有 "副作用": 函数的所有功能就是返回一个新的值，没有其他行为，并不会修改其他变量的值，提供同样的输入，函数总返回同样的结果.(引用透明)
  

## Why Java8
能兼容前面的版本，还提供了强大的新语汇和新设计模式.
把函数式编程一些最好的想法融入到了大家熟悉的Java语法中.
可以把函数式编程看作Java8中一个额外的设计模式.
```java
// List<Car> store
// Before
Collections.sort(store, new Comparator<Car>{
    public int compare(Car c1, Car c2) {
        return c1.getPrice().compareTo(c2.getPrice());
    }
})

// java 8
store.sort(comparing(Car::getPrice()))
```

## Java8 新特性
+ Iterable interface 中支持forEach()方法
```java
list.forEach(System::println);
```
+ 在接口中支持默认方法
+ 函数式接口与Lambda表达式
+ 对于集合框架来说, java Stream API 支持大量的并行操作
+ Java Time API(1.8开始改用这个) 
+ Collection API 提升
+ Concurrency API 提升 -> CompletableFuture 
+ Java IO 性能提升
+ Optinal 类 (解决空指针异常) 
```java 
public static Optional<Car> getCarOptional(int price, List<Car> store) {
    return store.stream()
        .filter(car -> car.getPrice() == 100)
        .findAny();
}
```

## Java中的函数
+ 编程语言中的函数通常是指方法，尤其是静态方法.
+ 在函数式编程中，方法和lambda 作为一等公民

## 方法引用
+ Java8新增的一种值的形式.
+ 例子: 将函数方法作为参数传递
```java
store.sort(comparing(Car::getPrice));
```  
+ java8的方法引用 ，以 :: 语法，把这个方法作为值进行传递
+ 与用对象引用传递对象类似（对象引用是指使用new 进行创建），在java8中，用::创建一个方法引用。

## 将函数作为值传递的另一种形式: Lambda -- 匿名函数
```java
// before
public static List<Car> filterElectricCars(List<Car> store) {
    List<Car> result = new ArrayList<>();
    for(Car car: store) {
        if("electric".equals(car.getPower())) {
            result.add(car);
        }
    }
    return result;
}

// java 8
public static boolean isElectricCar(Car car) {
    return "electric".equals(car.getPower());
}

public interface Predicate<T> {
    boolean execute(T t);
}

public static list<Car> filterCars(List<Car> store, Predicate<Car> p) {
    List<Car> result = new ArrrayList<>();
    for (Car car : store) {
        if(p.execute(car)) {
            result.add(car);
        }
    }
    return result;
}

// Usage
filterCars(store, Car::isElectricCar);

// java 8 改进版0.1
filterCars(store, (Car car) -> car.getPrice <100 && "electric".equals(car.getPower()));

// java 8 改进版2.0
public static <T> Collection<T> filter(Collection<T> c, Predicate<T> p);
filter(store, (Car a) -> a.getPrice() > 150); 
```

## 流处理
在Unix或Linux中，很多程序都从标准输入(Unix和C中的stdin, Java中的System.in)读取数据，然后把结果写入标准输出(Unix和C中的stdout, Java中的System.out)
例子:
`cat file1 file2 | tr "[A-Z]" "[a-z]" | sort | tail -10 `
用cat 命令把 两个文件连接起来创建一个流, 通过tr转换流中的字符, 使用sort对流中的行进行排序，最后tail -10 输出流的最后十行。这些操作都通过管道命令(|)连接在一起
类似的，Java 8 在java.util.stream中添加了一个Stream API;Strem 就是一系列T类型的流.
Stream API的很多方法可以链接起来形成一个复杂的流水线
```java
// before java8
Map<Currency, List<Transaction>> transactionsByCurrencies = new HashMap();
for(Transaction transaction: transactions) {
    if(transaction.getPrice() > 1000) {
        Currency currency = transaction.getCurrency();
        List<Transaction> transactionsForCurrency = transactionsByCurrencies.get(currency);
        if (transactionsForCurrency == null) {
            transactionsForCurrency = new ArrayList<>();
            transactionsByCurrencies.put(currency, transactionsForCurrency);
        }
        transactionsForCurrency.add(transaction);
    }
}
// java 8
import static java.util.stream.Collectors.toList;
Map<Currency, List<Transaction>> transactionsByCurrencies = 
    transactions.stream()
        .filter((Transaction t) -> t.getPrice() > 1000)
        .collect(groupingBy(Transaction::getCurrency));

// 顺序处理
List<Car> LuxuryCars = store.stream()
                .filter((Car c) -> c.getPrice() > 100)
                .collect(Collectors.toList());

// 并行处理
List<Car> LuxuryCars = store.parallelStream()
            .filter((Car c) -> c.getPrice() > 100)
            .collect(Collectors.toList());
```

## 默认方法
+ Java8 中加入默认方法主要是为了支持库拓展，让jdk库工程师能够写出更容易改进的接口
+ 有很多替代集合框架都用Collection API 实现了接口. 现在如果要给接口加入一个新方法，意味着所有的实体类都必须为其提供一个实现，这是不可能的
+ 如何改变已发布的接口而不破坏已有的接口呢?
+ 在Java8中接口可以包含实现类没有提供实现的方法签名.缺失的方法主体随接口提供了,而不是由实现类提供

 ```java
 default void sort(Comparator<? super E> c) {
     Collections.sort(this, c);
 }
 ``` 

 ## Optional类
 在Java 8 里有一个Optional类，使用它的话，就可以帮助避免出现空指针异常
 Optional 时一个容器对象，可以包含,也可以不包含一个值。Optional中有方法来明确处理值不存在的情况，这样就可以避免NullPointer异常了.