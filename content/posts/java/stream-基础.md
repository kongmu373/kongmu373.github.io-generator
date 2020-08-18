---
title: "Stream-常见使用"
date: 2020-07-02T16:00:20+08:00
draft: true
tags: ["stream"]
series: ["Java体系"]
categories: ["Java体系"]
---

本文章主要介绍了Stream使用的常见方法以及Java样例代码.

## 常见使用
----
### 筛选过滤
+ filter方法
接受一个判定(Predicate),并返回一个符合判定的元素流
```java
List<Car> carList = store.stream()
	.filter(Car::isElectric)
	.collect(toList());
```
+ distinct方法
返回一个没有重复的元素流(用元素的equals方法来判断)
```java
List<Integer> number = Arrays.asList(1, 2, 3, 3, 5, 5);
numbers.stream()
	.distinct()
	.forEach(System.out::println)
```

+ limit方法
返回一个不超过给定长度的流。所需的长度作为参数传递给limit。
如果流是有序的，则最多返回前n个元素。
```java
List<Car> carList = store.stream()
			.filter(c-> c.getPrice() > 100)
			.limit(3)
			.collect(toList());
```

+ skip方法
返回一个扔掉了前n个元素的流.如果流中元素不足n个，则返回一个空流
```java
List<Car> carList = store.stream()
			.filter(c->c.getPrice()>100)
			.skip(2)
			.collect(toList());
```

### 映射(map,flatMap)
从元素流中选择某些信息，并将其映射成一个新元素流

+ map方法
接收一个函数(Functional)作为参数，这个函数应用到每个元素上,
并映射成一个新元素
```java
List<String> carList = store.stream()
			.map(Car::getName)
			.collect(toList());
```

+ flatMap方法
合并，如:输入单词列表["Hello","World"], 返回列表["H","e"...]
```java
words.stream()
	.map(word -> word.split("")) // stream<String[]>
	.distinct()
	.collect(toList());
words.stream()
	.map(word -> word.split(""))
	.map(Arrays::stream) // Stream<String[]> -> Stream<Stream<string>>
	.distinct()
	.collect(toList());
List<String> uniqueCaracters = words.stream()
		.map(w-> w.split(""))
		.flatMap(Arrays::stream)  // 将每个元素换成流 进行合并
		.distinct()
		.collect(Collectors.toList());
```

### 查找和匹配

+ anyMatch方法
检查元素六种是否有一个元素匹配判定(Predicate) 返回boolean
```java
if(store.stream().anyMatch(c->c.getPrice() > 100)) {
    System.out.println("There is the luxury car");
}
```

+ allMatch方法
检查流中是否全部元素都能匹配给定的判定(Predicate) 返回boolean
```java
boolean isOil = store.stream()
                    .allMatch(c -> "oil".equals(car.getPower()));
```

+ noneMatch方法
检查流中是否全部元素都不能匹配给定的判定(Predicate) 返回boolean 
```java
boolean noneOil = store.stream()
                    .noneMatch(c -> "oil".equals.(car.getPower()));
```

+ findAny方法
返回当前流中的任意元素，通常与filter搭配使用
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> numOptional = nums.stream()
                                .map(x->x*x)
                                .filter(x-> x%3 == 0)
                                .findAny();
```

+ findFirst方法
```java
List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5);
Optional<Integer> numOptional = nums.stream()
                                .map(x->x*x)
                                .filter(x-> x%3 == 0)
                                .findFirst();
```

+ 注意:
  + 像noneMatch,findFirst,findAny以及limit的操作都是短路操作,不用处理整个流就能得到结果.
  + 如果碰到无限流的时候，短路求值(Short Cutting)就能将其变成有限流


### 归约(reduce)
reduce须臾接受两个参数:
1. 一个初始值
2. 一个Lambda表达式把两个流元素结合起来并产生一个新值
```java
int sum = 0;
for (int x : sum) {
    sum += x;
}

// 等价于
int sum = nums.stream().reduce(0, (a, b) -> a+b);
// 无初始值，只有Lambda表达式的方法
Optional<Integer> sum = nums.stream().reduce((a,b) -> a+b);
Optional<Integer> max = nums.stream().reduce(Integer::max);

```

### 数值流
+ 原始类型流特化
三大特化接口: IntStream, DoubleStream 和 LongStream分别将流中的元素特化为int, double 和long.
目的: 避免了装箱成本

+ 将普通流Stream转化到数值流
将流转换为特化版本: mapToInt, mapToDouble 和 mapToLong
```java
int sum = store.stream()
            .mapToInt(Car::getPrice) // .mapToLong mapToDouble
            .sum();
```

+ 数值流转化到普通流Stream
boxed()
```java
IntStream intStream = store.stream().mapToInt(Car::getPrice);
Stream<Integer> stream = intStream.boxed();
```

+ 产生有范围的数值流
range方法(不含右边界)
```java
IntStream evenNumbers = IntStream.range(1, 100)
							.filter(n -> n%2 == 0);
System.out.println(evenNumbers.count());
```
+ rangeClose方法(包含右边界)
```java
IntStream evenNumbers = IntStream.rangeClose(1, 100)
							.filter(n -> n%2 == 0);
System.out.println(evenNumbers.count());
```

## 构建流的方法
+ 从值构建流
```java
Stream<String> fruits = Stream.of("Apple", "Orange", "Pear");
Stream<String> emptyStream = Stream.empty();
```

+ 由数组/集合创建流
```java
Arrays.stream(nums); // 数组
Collection.stream(); // 集合
```

+ 从文件创建流
java.nio.file.Files 中的很多静态方法都会返回一个流
Files.lines: 返回一个由指定文件中的各行构成的字符串流
```java
long uniqueWords = 0 ;
try(Stream<String> lines = Files.lines(Paths.get("data.csv"), 
Charset.defaultCharset())) {
	uniqueWords = lines.flatMap(line -> Arrays.stream(line.split("")))
	.distinct()
	.count();
}
catch(IOException e) {
	// ...
}
```

+ 从函数创建流
Stream API提供了两个静态方法来从函数生成流: Stream.iterate和Stream.generate.
这两个操作可以创建所谓的无限流: 不像固定集合创建的流那样由固定大小的流
迭代: iterate
```java
Stream.iterate(0, n->n*2)
.limit(2)
.forEach(System.out::println);
```
生成: generate
```java
Stream.generate(Math::random)
.limit(5)
.forEach(System.out::println);
```