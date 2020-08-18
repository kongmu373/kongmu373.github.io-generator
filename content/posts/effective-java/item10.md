---
title: "Item10"
date: 2020-06-14T17:21:00+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---

## Item 10: Obey the general contract when overriding equals（覆盖 equals 方法时应遵守的约定）
> 覆盖 equals 方法似乎很简单，但是有很多覆盖的方式会导致出错，而且后果可能非常严重。

## `equals` 约定 -- equivalence relation
+ 覆盖`equals` 应该遵守(equivalence relation)
  + 自反性(Reflexive), `x.equals(x) == true`
  + 对称性(Symmetric), `x.equals(y) == y.equals(x)`
  + 传递性(Transitive), `x.equals(y) and y.equals(z) == x.equals(z)`
  + 一致性(Consistent), 连续调用 `x.equals(y)` 应该返回相同的值
  + `x.equals(null) == false` (x不为null)

## 其他注意事项
+ 无法继承一个可实例化的类并添加一个值组件，同时保留 `equals` 约定。
+ 不能再覆盖`equals`时依赖无关，不可靠的资源
+ 覆盖`equals`的同时，一定要覆盖`hashCode`
+ 在覆盖`equals`不要替换参数类型,以致于函数变成了重载(overload) 而不是(override).(当然地，无脑使用@Override就能(to be safe)).
+ 总之,除非必须，否则不要覆盖 equals 方法.
+ 避免手写，一个很好的替代方法是使用谷歌的开源 AutoValue 框架，或者一个很好的替代方法是使用谷歌的开源 AutoValue 框架,IDE 也有生成 `equals` 和 `hashCode` 方法的功能.
+ 当然，AutoValue更好,使用AutoValue看这篇[文章](https://zhuanlan.zhihu.com/p/24193465)