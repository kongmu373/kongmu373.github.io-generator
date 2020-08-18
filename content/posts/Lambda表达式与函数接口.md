---
title: "Lambda表达式与函数接口"
date: 2020-05-26T14:05:22+08:00
draft: true
tags: ["Java8"]
series: ["Java8"]
categories: ["Java8"]
---

# Lambda表达式与函数式接口

## Lambda表达式
1. Lambda 是什么? 
	+ 可以把Lambda表达式理解为 简洁地表示可传递匿名函数的一种方式
	+ Lambda表达式没有名称，但它有参数列表，函数主体，返回类型，有时还可以抛出的异常列表。
	+ 有以下特性:
		1. 匿名:与匿名函数一样不需要方法名
		2. 函数:有参数列表，函数主题，返回类型，以及抛出异常
		3. 灵活传递:可以作为参数或者存储在变量中
		4. 简洁：相比匿名函数代码量更少
   
2. Lambda 如何使用呢?
```java
// 参数列表 	箭头	函数体
（int x, int y)  -> 	x + y 

// 传统写法
int sum(int x, int y) {
	return x + y;

// 只有一条语句
(String s) -> s.length()
(Apple a) -> a.getWeight() > 10 

// 多条语句
(int x, int y) -> {
	System.out.println("Result:");
	system.out.println(x+y);
	return x + y;
}
// 无参
() -> 42

// 省去参数类型
( a1,  a2) -> a1.getWeight().compareTo(a2.getWeight())
```
   
3. 为什么用Lambda表达式?
最直观的好处就是使得代码变得异常简洁

## 函数式接口
1. 函数式接口定义
   + 只定义一个抽象方法的接口
   + java8以后接口现在还可以拥有默认方法.
   + 哪怕有很多默认方法，只要只定义了一个抽象方法，它就仍然是一个函数式接口
2. 函数描述符 
   +  函数式接口的抽象方法的签名基本上就是Lambda表达式的签名，将这种抽象方法叫做函数描述符
+ 例子: 函数式接口与对应的函数描述符
```java
// 例子:
// java.util.Comparator 函数描述符: (T, T) -> int
public interface Comparator<T> {
	int compare(T o1, T o2);
}

```

## Java 8 中常见函数式接口
1. Predicate (判定) 
java.util.function.Predicate 接口定义了一个名叫test的抽象方法,它接受泛型T对象,并返回一个boolean.
如果需要表示一个涉及类型T的布尔表达式时，就可以使用这个接口了
```java
@FunctionalInterface
public interface Predicate<T> {
	 boolean test(T t);
}

// 用法
public staitc <T> List<T> filter(List<T> list, Predicate<T> p) {
	List<T> results = new ArrayList<>();
	for(T s: list) {
		if(p.test(s)) {
			results.add(s);
		}
	}
}

Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

2. Consumer(消费)
java.util.function.Consumer 定义了一个名叫acccept的抽象方法，它接受泛型T的对象，没有返回值(void).
你如果需要访问类型T的对象，并对其执行某些操作，就可以使用这个接口
```java
@FunctionalInterface
public interface Consumer<T> {
	void accept(T t);
}

public static <T> void forEach(List<T> list, Conseumer<T> c) {
	for (T t: list) {
		c.accept(i);
	}
}
//Lambda时Consumer中accept方法的实现
forEach(Arrays.asList(1,2,3,4,5), (Integer i) -> {System.out.println(i);})
```

3. Function 
java.util.funciton.Function<T,R>接口定义了一个叫作apply的方法,它接受一个泛型T的对象，并返回一个泛型R的对象。
如果你需要定义一个Lambda,将输入对象映射到输出，就可以使用这个接口
```java

@FunctionalInterface
public interface Function<T, R> {
	R apply(T t);
}

public static List<R> map(List<T> list, Function<T, R> f) {
	List<R> result = new ArrayList<>();
	for(T s: result) {
		result.add(f.apply(s));
	}
	return result;
}

// 将一个String列表映射到包含每个String长度的Integer列表
List<Integer> list = map(Arrays.asList("labdas","in","action"), (String s) -> s.length());
```

4. Supplier
	java.util.function.Supplier接口定义了一个叫作get的方法, 没有传入参数，但返回一个泛型T的对象。
	如果你需要产生某个类型的对象时，就可以使用这个接口
```java
@FunctionalInterface
public interface Supplier<T> {
	/*
	 * Gets a result.
	 * @return a result.
	 */
	T get();
}

Supplier<Apple> s = Apple::new; // () -> new Apple();
Apple a = s.get();
```

## 用于原始类型的特殊函数式接口
```java
public interface IntPredicate {
	boolean test(int i);
}
IntPredicate evenNumbers = (int i) -> i%2 == 0;
evenNumbers.test(1000); // true

Predicate<Integer> oddNumbers = (Integer i) -> i%2 == 1;
oddNumbers.test(1000); // false 
```
针对专门的输入参数类型的函数式接口的名称要加上对应的原始类型前缀即可得到用于基本类型的特殊函数式接口

## 方法引用
方法引用让你可以重复使用现有的方法定义并传递这些方法
| Lambda表达式 | 等同的方法引用 |
| ---- | ----|
| (Car c) -> c.getPrice() | Car::getPrice |
| (String s) -> System.out.println(s) | System.out.::println|

## 如何构建方法引用
1. 指向静态方法的方法引用(例如Integer的parseInt方法, 写作Integer::parseInt)
2. 指向任意类型实例方法的方法引用(例如String的length方法，写作String::length)
3. 指向现有对象的实例方法的方法引用(假设有一个局部变量t用于存放Transaction类型的对象，它支持实例方法getVlaue,就可以写t::getValue)

## 构造函数引用
对于一个现有构造函数，可以利用它的名称和关键字new来创建它的一个引用ClassName::new.它的功能与指向静态方法的引用类似
例子:
```java
// 无参构造器: car()
Supplier<Car> s = Car::new;
Car c1 = s.get();

// 一个价格参数构造器: Car(Integer price)
Function<Integer, Car> fn = Car::new;
Car c2 = fn.apply(100);

// Car(Integer price, String color)
BiFunction<Integer, String, Car> biFn = Car::new;
Car c3 = biFn.apply(100, "red");
```

## Lambda表达式类型检查与推断
1. 类型检查
Lambda的类型是从使用Lambda的上下文推断出来的
上下文(接受传递方法的参数或变量) 中 Lambda表达式需要的类型称为目标类型
```java
// cars是接受传递方法的变量，cars的类型即为目标类型
List<Car> cars = filter(store, (Car c) -> c.getPrice() > 100)
```

## 类型推断
Java编译器会从目标类型推断出用什么函数式接口来配合Lambda表达式, 所以也可以推断出适合Lambda的签名,引物函数描述符可以通过目标类型来得到.
```java
// filter -> 找到第一个匹配的就返回
Car car = filter(store, (Car c) -> c.getPrice() > 100);
Car car = filter(store, c-> c.getPrice() > 100);

Comparator<Apple> c = (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
Comparator<Apple> c = (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());
```

