---
title: "Item14"
date: 2020-06-19T14:56:53+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 14: Consider implementing Comparable（考虑实现 Comparable 接口）


## Comparable实现的例子
+  Single-field Comparable with object reference field
```java
public final class CaseInsensitiveString
    implements Comparable<CaseInsensitiveString> {
        private final String s;
        public CaseInsensitiveString(String s) {
            this.s = Objects.requiredNonNull()
        }
        ...

        public int compareTo(CaseInsensitiveString cis) {
            return String.CASE_INSENSITIVE_ORDER.compare(s, cis);
        }

        public static void main(String[] args) {
            Set<CaseInsensitiveString> s = new TreeSet<>();
            for (String arg : args) {
                s.add(new CaseInsensitiveString(arg));
            }
            System.out.println(s);
        }
    }
```
+ Multiple-field Comparable with object reference field
```java
public final class PhoneNumber implements Cloneable, Compareable<PhoneNumber> {
    private final short areadCode, prefix, lineNum;

    public PhoneNumber(int areaCode, int prefix, int lineNum) {
        this.areaCode = rangeCheck(areaCode, 999, "area code");
        this.prefix = rangeCheck(areaCode, 999, "prefix");
        this.lineNum = rangeCheck(areaCode, 999, "lineNum");
    }

    private static short rangeCheck(int value, int max, String message) {
        if(val < 0 || val > max) {
            throw new IllegalException(message + " : " + val);
        }
        return (short) val;
    }
    private static final Comparator<PhoneNumber> COMPARATOR = comparaingInt((PhoneNumbert pn) -> pn.areaCode)
    .thenComparaingInt(pn -> pn.prefix)
    .thenComparaingInt(pn -> pn.lineNum);

    public int compareTo(PhoneNumber ph) {
        return COMPARATOR.compare(this, ph);
    }

    private static PhoneNumber randomPhoneNumber() {
        Random rnd = ThreadLocalRandom.current();
        return new PhoneNumber((short) rnd.nextInt(1000),
                               (short) rnd.nextInt(1000),
                               (short) rnd.nextInt(10000));
    }

    public static void main(String[] args) {
        NavigableSet<PhoneNumber> s = new TreeSet<PhoneNumber>();
        for (int i = 0; i < 10; i++)
            s.add(randomPhoneNumber());
        System.out.println(s);
    }
}
```

## 总结
+ `Comparable`接口表面对象是自然排序
+ `Comparable`实现的类在多种通用算法种使用
+ `compareTo`应该遵守`equals`一样的规则
+ `compareTo`应该返回`-1`,`0` and `1`以及不能integer溢出
  对于不是自然排序以及无能力实现`Comparable`接口的，应使用`Comparator`.
