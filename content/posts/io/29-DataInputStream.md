---
title: "29 DataInputStream"
date: 2020-07-23T15:06:27+08:00
draft: true
categories: ["IO"]
---
[原文链接](http://tutorials.jenkov.com/java-io) 作者：Jakob Jenkov


## Java DataInputStream Example
```java
DataInputStream dataInputStream = new DataInputStream(
                            new FileInputStream("binary.data"));

int    aByte   = input.read();
int    anInt   = input.readInt();
float  aFloat  = input.readFloat();
double aDouble = input.readDouble();
//etc.

input.close();
```

## Create a DataInputStream
```java
DataInputStream dataInputStream =
        new DataInputStream(
                new FileInputStream("data/data.bin"));
```

## Using a DataInputStream With a DataOutputStream
```java
public class DataInputStreamExample {

    public static void main(String[] args) throws IOException {
        DataOutputStream dataOutputStream =
            new DataOutputStream(
                new FileOutputStream("data.bin"));

        dataOutputStream.writeInt(123);
        dataOutputStream.writeFloat(123.45F);
        dataOutputStream.writeLong(789);

        dataOutputStream.close();

        DataInputStream dataInputStream =
            new DataInputStream(
                new FileInputStream("data.bin"));

        int int123 = dataInputStream.readInt();
        float float12345 = dataInputStream.readFloat();
        long long789 = dataInputStream.readLong();

        dataInputStream.close();

        System.out.println("int123     = " + int123);
        System.out.println("float12345 = " + float12345);
        System.out.println("long789    = " + long789);
    }
}
```

## Read boolean
`boolean myBoolean = dataInputStream.readBoolean();`

## Read byte
`byte myByte = dataInputStream.readByte();`

## Read Unsigned byte
+ 因为返回的是无符号byte，超过127个字节所以用int存
`int myUnsignedByte = dataInputStream.readUnsignedByte();`

## Read char
`char myChar = dataInputStream.readChar();`

## Read double
`double myDouble = dataInputStream.readDouble();`

## Read float
`float myFloat = dataInputStream.readFloat();`

## Read  short
`short myShort = dataInputStream.readShort();`

## Read Unsigned short
`int myUnsignedShort = dataInputStream.readUnsignedShort();`

## Read int
`int   myInt = dataInputStream.readInt();`

## Read long
`long   myLong = dataInputStream.readLong();`

## Read UTF
+ 从DataInputStream中读取Java字符串,预期该数据将以UTF-8编码
`String   myString = dataInputStream.readUTF();`

## Closing a DataInputStream
+ 调用close()方法
+ 使用try-with-resources