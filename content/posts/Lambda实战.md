---
title: "Lambda实战"
date: 2020-05-29T09:08:47+08:00
draft: true
tags: ["Java8"]
series: ["Java8"]
categories: ["Java8"]
---

## 如何利用Lambda简化接口代码
接口实现简化
```java
public interface OperationInterface {
    
    int operator(int a, int b); // (int a, int b) -> int -- (int, int) ->int 
}

public class AddOperation implements OperationInterface {
    @Override 
    public int operator(int a, int b) {
        return a+b;
    }
}

public class MultiplicationOperation implements Operatinable {
    @Override
    public int operator(int a, int b) {
        return a * b;
    }
}

public class OperationDemo {

    public static void main(String[] args) {
        Operatinable multiplicationOperation = new MultiplicationOperation();
        System.out.println("Multiplication Operation: " + multiplicationOperation.operator(1, 3));
        Operatinable AddOperation = new AddOperation();
        System.out.println("AddOperation: " + AddOperation.operator(1, 3)); 
        Operatinable op = (a, b) -> a * a + b * b;
        System.out.println(" Lambda Operation: " + op.operator(1, 3));
    }
}

```

## 实例: 优化比较器

```java
// before
public class CarComparator implements Comparator<Car> {
    public int compare(Car c1, Car c2) {
        return c1.getPrice().compareTo(c2.getPrice());
    }
}
store.sort(new CarComparator());

// 使用匿名类
store.sort(new Comparator<>() {
    public int compare(Car c1, Car c2) {
        return c1.getPrice().compareTo(c2.getPrice());
    }
})

// 使用Lambda表达式, 函数描述符为 (T, T) -> int
store.sort((Car c1, Car c2) -> c1.getPrice().compareTo(c2.getPrice()))
store.sort(c1, c2) -> c1.getPrice().compareTo(c2.getPrice()));

// 使用方法引用
store.sort(comparing(Car::getPrice))
store.forEach(System.out::println)
```

## 复合 
```java
// 比较器复合 
store.sort(Comparing(Car::getPrice).thenComparing(Car::getWeight).reversed())

// Predicate复合
Predicate<Car> oilCar =  c -> "oil".equals(c.getPower());
// Predicate<Car> nonOilCar = c -> !"oil".equals(c.getPower());
// !
Predicate<Car> nonOilCar = oilCar.negate();
// && ||
Predicate<Car> selectedCar = nonOilCar.and(c -> c.getPrice() < 100).or(c->"wind".equals(c.getPower()));

// 函数接口复合 
Function<Integer, Integer> f = x -> x + 1; //  2
Function<Integer, Integer> g = x -> x * 2; // 2*2 = 4
// f.compose(g)  先调用g, 再调用f
Function<Integer, Integer> h = f.andThen(g);  // 先调用f, 再调用g
sout(h.apply(1)); // 4
```

## Lambda表达式再设计模中的应用
1. 工厂模式
   + 什么是工厂模式?
     + 使用工厂模式，无需向调用者暴露实例化的逻辑就能完成实例的创建
     + 会通过定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行
   + 优缺点
     + 优点:
       + 调用者想创建一个对象，只要知道名称就可以了
       + 扩展性高，要增加一个类的对象，只需扩展它的工厂类就姓
       + 屏蔽具体实现
     + 缺点：
       + 每次增加一个类的对象，都需要增加一个具体类和对象实现工厂
       + 在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖
```java
// before
public class ShapeFactory {
    // private static final String CIRCLE = "circle";

    public static Shape createShape(String name) {
        switch(name) {
            case "CIRCLE":return new Circle();
            case "RECTANGLE": return new Rectangle();
            case "SQUARE": return new Square();
            case "TRIANGLE": return new Triangle();
            default:
                throw new RuqntimeException("No such shape" + name );
        }
    }
}
Shape p = ShapeFactory.Shape("CIRCLE");

// lambda
Supplier<Shape> CircleSupplier = Circle::new;
Shape c = CircleSupplier.get();

final static Map<String, Supplier<Shape>> map = new HashMap<>();
static {
    map.put("CIRCLE", Circle::new);
    map.put("RECTANGLE", Rectangle::new);
    map.put("SQUARE", Square::new);
    ...
}

public static Shape createShape(String name) {
    Supplier<Shape> s = map.get(name);
    if(s!=null) return s.get();
    throw new IllegalArgumentException("No such shape " + name);
}
```

## 策略模式
1. 什么是策略模式
   1. 完成一项任务，往往可以有多种不同的方式，每一种方式称为一个策略，我们可以根据环境或者条件的不同选择不同的策略来完成该项任务，可以灵活地增加或减少策略
   2. 在有多种策略相似的情况下，if ... else 所带来的复杂变得难以维护
2. 优缺点
   1. 优点  
      1. 策略可以自由切换
      2. 避免使用多重条件判断
      3. 扩展性良好

    2. 缺点
       1. 策略类型会增多
       2. 所有策略都需要对外暴露
3. 实现
   1. 策略模式代表了解决一类算法的通用解决方案,你可以在运行时选择使用哪种方案
   2. 策略模式包含三部分内容:
      1. 一个代表某个算法的接口 (它是策略模式的接口)
      2. 一个或者多个该接口的具体实现，它们代表了算法的多种实现
      3. 一个或多个使用策略对象的客户
```java
public interface ValidationStrategy{
    boolean execute(String s);
}

public class IsAllLowerCase implements ValidationStrategy {
    public boolean execute(String s) {
        return s.matches("[a-z]+");
    }
}
public class IsNumeric implements ... {
    public boolean execute(String s) 
    {
        return s.matches("\\d+");
    }
}
...
public class Validator{
    private final ValidationStrategy strategy;
    public Validator(ValidationStrategy v) {
        this.strategy = v;
    }
    public boolean validate(String s) {
        return strategy.execute(s);
    }
}

// before
Validator numericValidator = new Validator(new IsNumeric());
numericValidator.validate("aaaa");

// lambda
Validator lowerCaseValidator = new Validator((String s) -> s.matches("[a-z]+");)
```