---
title: "Item6"
date: 2020-06-12T16:27:48+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item6 Avoid creating unecessary objects(避免创建不必要的对象)


## 重用对象从而提高性能
+ exmaple1:
```java
// Reusing expensive object for improved performance (Pages 22 and 23)
public class RomanNumerals {
    // Performance can be greatly improved! (Page 22)
    static boolean isRomanNumeralSlow(String s) {
        return s.matches("^(?=.)M*(C[MD]|D?C{0,3})"
                + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
    }

    // Reusing expensive object for improved performance (Page 23)
    private static final Pattern ROMAN = Pattern.compile(
            "^(?=.)M*(C[MD]|D?C{0,3})"
                    + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");

    static boolean isRomanNumeralFast(String s) {
        return ROMAN.matcher(s).matches();
    }

    public static void main(String[] args) {
        int numSets = Integer.parseInt(args[0]);
        int numReps = Integer.parseInt(args[1]);
        boolean b = false;

        for (int i = 0; i < numSets; i++) {
            long start = System.nanoTime();
            for (int j = 0; j < numReps; j++) {
                b ^= isRomanNumeralSlow("MCMLXXVI");  // Change Slow to Fast to see performance difference
            }
            long end = System.nanoTime();
            System.out.println(((end - start) / (1_000. * numReps)) + " μs.");
        }

        // Prevents VM from optimizing away everything.
        if (!b)
            System.out.println();
    }
}
```

+ example2:
```java
// Hideously slow program! Can you spot the object creation? (Page 24)
public class Sum {
    private static long sum() {
        Long sum = 0L;
        for (long i = 0; i <= Integer.MAX_VALUE; i++)
            sum += i;
        return sum;
    }

    public static void main(String[] args) {
        int numSets = Integer.parseInt(args[0]);
        long x = 0;

        for (int i = 0; i < numSets; i++) {
            long start = System.nanoTime();
            x += sum();
            long end = System.nanoTime();
            System.out.println((end - start) / 1_000_000. + " ms.");
        }

        // Prevents VM from optimizing away everything.
        if (x == 42)
            System.out.println();
    }
}
```

## 总结
+ `"hello"` 比用 `new String("hello")` 要好
+ `Boolean.valueOf("true")` 比 `new Boolean("true")`要好
+ (Immutable object)不可变对象应该重用
+ (Mutable object)可变对象如果你确定它不会改变，也应该重复使用
+ 使用基本类型比装箱类型效率更高
+ 留意隐式的自动装拆箱(hidden autoboxing)
+ 延迟加载并没有明显的性能改善，反而会使实现复杂化