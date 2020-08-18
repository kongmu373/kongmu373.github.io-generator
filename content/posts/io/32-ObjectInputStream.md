---
title: "32 ObjectInputStream"
date: 2020-07-23T15:23:21+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io) 作者：Jakob Jenkov

## ObjectInputStream Example
```java
ObjectInputStream objectInputStream =
    new ObjectInputStream(new FileInputStream("object.data"));

MyClass object = (MyClass) objectInputStream.readObject();
//etc.

objectInputStream.close();
```

## Using an ObjectInputStream With an ObjectOutputStream
```java
package com.kongmu373.leetcode.editor.en;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class ObjectInputStreamExample {
    public static class Person implements Serializable {
        public String name = null;
        public int age = 0;
    }

    public static void main(String[] args) throws Exception {
        FileOutputStream fileOutputStream = new FileOutputStream("person.bin");
        ObjectOutputStream outputStream = new ObjectOutputStream(fileOutputStream);
        Person person = new Person();
        person.name = "lili";
        person.age = 11;
        outputStream.writeObject(person);
        outputStream.close();

        // input
        ObjectInputStream inputStream =
            new ObjectInputStream(new FileInputStream("person.bin"));
        Person o = (Person) inputStream.readObject();
        inputStream.close();
        System.out.println(o.name);
        System.out.println(o.age);
    }
}

```

## Closing a ObjectInputStream
```java

// 调用close方法
objectInputStream.close();

// try-with-resources
InputStream input = new FileInputStream("data/data.bin");

try(ObjectInputStream objectInputStream =
    new ObjectInputStream(input)){

    Person personRead = (Person) objectInputStream.readObject();
}
```

