---
title: "Item4 5"
date: 2020-06-12T16:01:33+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item4 Enforce noninstantiability with a private constructor(强迫非实例化的类使用私有构造方法)
```java
// Noninstantiable utility class
public class UtilityClass {
    // Suppress(禁用) default constructor for noninstantiability.
    private UtilityClass() {
        throw new AssertionError();
    }

    // Remainder omitted
}
```

### 总结
+ 使用私有构造方法避免实例化该类
+ 如果调用该私有方法就抛出异常
+ 这种技术在构建工具类的时候，经常会被用到

## Item5 Prefer dependency injection to hardwiring resources (依赖注入优于硬连接资源)

### 不适当使用静态工具类
+ 在工具类实现依赖于一个或多个底层资源的类,以后代码行为都不适合(它们假设只使用一个字典。在实际应用中，每种语言都有自己的字典，特殊的字典用于特殊的词汇表)
```java
// Inappropriate use of static utility
public class SpellChecker {
    private static final Lexicon dictionary = ...;
    private SpellChecker() {} // Noninstantiable
    public static boolean isValid(String word) { ... }
    public static List<String> suggestions(String typo) { ... }
}


// Inappropriate use of singleton - inflexible & untestable!
public class SpellChecker {
    private final Lexicon dictionary = ...;
    private SpellChecker(...) {}
    public static INSTANCE = new SpellChecker(...);
    public boolean isValid(String word) { ... }
    public List<String> suggestions(String typo) { ... }
}
```

### 依赖注入
> 将创建它们的资源或工厂传递给构造函数（或静态工厂或构建器）。这种操作称为依赖注入

```java
// Dependency injection provides flexibility and testability
public class SpellChecker {
    private final Lexicon dictionary;
    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }
    public boolean isValid(String word) { ... }
    public List<String> suggestions(String typo) { ... }
}

// 或者将资源工厂传递给构造函数

public SpellChecker(Supplier<? extends Lexicon> LexiconFactory){...}
```

### 总结
+ 不要使用单例或静态实用工具类来实现依赖于一个或多个底层资源的类
+ 也不要让类直接创建这些资源
+ 将创建它们的资源或工厂传递给构造函数（或静态工厂或构建器）,也即是依赖注入
+ 当你面对的是大型项目，依赖数目巨大时，使用依赖注入框架（如 Dagger、Guice 或 Spring）
