---
title: "Item13"
date: 2020-06-18T16:13:31+08:00
draft: true
tags: ["Effective Java"]
series: ["Effective Java"]
categories: ["Effective Java"]
---


## Item 13: Override clone judiciously（谨慎覆盖 clone 方法）


## Cloneable接口
+ 跟Serializable接口一样，只是声明该对象可以被克隆，具体行为由类设计者决定。
+ 如果一个类实现了Cloneable，Object的clone方法就返回该对象的拷贝，否则就会抛出CloneNotSupportedException异常。
+ 要求实现类和父类必须提供一个Clone方法，在Clone方法中调用super.clone方法

## Clone方法的通用约定
+ 对于任何对象x， x.clone()!=x ， x.clone().getClass() == x.getClass(), x.clone.equals(x)将会是true

## 实现良好的clone方法
```java
...
    // Clone method for class with no references to mutable state (Page 59)
    @Override public PhoneNumber clone() {
        try {
            return (PhoneNumber) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();  // Can't happen
        }
    }


        // Clone method for class with references to mutable state
    @Override public Stack clone() {
        try {
            Stack result = (Stack) super.clone();
            result.elements = elements.clone();
            return result;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }

    //deep copy


	@Override public HashTable clone(){
		try{
			HashTable result = (HashTable) super.clone();
			result.buckets = new Entry[buckets.length];
			for(int i=0; i<buckets.length; i++){
				if(buckets[i]!=null)
				result.buckets[i] = buckets[i].deepCopy();
			}
			return result;
		}catch(CloneNotSupportedException e){
			throw new AssertionError();
		}
	}
```

## 总结
+ `Cloneable` is broken.
+ 如果你想覆盖`clone`方法但不是`final`类，你应该返回一个调用了`super.clone`的对象
+ 实现`Cloneable`接口的类要提供一个public的`clone`方法
+ `clone`不应该破坏原来（被复制）的对象
+ 默认`clone`是浅拷贝，可以自行实现深拷贝
+ 更好的复制应该提供copy构造器或者copy工厂方法去代替`clone`方法

