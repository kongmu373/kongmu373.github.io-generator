---
title: "Item3"
date: 2020-06-10T16:16:08+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 3:Enforce the singleton property with a private consturctor or an enum type 

> 使用私有构造函数或枚举类型实施单例属性

### Singleton with public final field (使用final字段实现单例)
+ 不好使用单元测试
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();

    private Elvis() {}

    public void leaveTheBuiding() {
        // do something
    }

    // This code would normally appear outside the class!
    public static void main(String[] args) {
        Elvis elvis = Elvis.INSTANCE;
        elvis.leaveTheBuilding();
    }
}
```

## Single with static factory
```java
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis() {}
    public static Elvis getInstance(){
        return INSTANCE;
    }
    public void leaveTheBuiding() {
        // do something
    }

    // This code would normally appear outside the class!
    public static void main(String[] args) {
        Elvis elvis = Elvis.getInstance();
        elvis.leaveTheBuilding();
    }
}
```

## Enum singleton - the preferred approach
```java
public enum Elvis {
    INSTANCE;
    
    public void leaveTheBuiding() {
        // do something
    }

    // This code would normally appear outside the class!
    public static void main(String[] args) {
        Elvis elvis = Elvis.INSTANCE;
        elvis.leaveTheBuilding();
    }
}
```

## 总结
注意:讨论的单例不包括懒加载(lazy initialization)
+ 为了避免反射调用构造方法，在私有构造方法中抛出一个异常
+ 如果需要标准序列化(seerialization) 让所有字段都使用`transient`，以及覆盖`readResolve`方法
+ 最好单例实现是使用单个元素的enum

