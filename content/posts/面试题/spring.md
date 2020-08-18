---
title: "Spring"
date: 2020-07-08T17:09:31+08:00
draft: true
tags: ["面试题"]
series: ["面试题"]
categories: ["面试题"]
---

## 什么是IoC容器， 为什么需要IoC
> 背景:一个应用要启动，可能有不同的组件,把不同职责的功能放在不同的类里面，类之间互相依赖,随着应用规模越来越大，这种依赖就越来越繁杂
+ IoC,Inverse of Control(控制反转) 不需要手动控制各种对象的创建和依赖,(由IoC容器完成)将依赖的装配权交给IoC容器 例子:
```java
class UseService {
// UserService 依赖于 UserDao,如果我们没有IoC，就需要手动去装配
    private UserDao = new UserDao();
// 而使用了IoC，我们只需要声明它们的依赖关系，将实际装配交给IoC容器
    @Autowired
    private UserDao;
}
class UserDao {

}

```


## AOP与AOP原理
+ AOP - Aspect Oriented Programming(面向切面编程)
  + 什么是AOP?
    + 在运行时，动态地将代码切入到类的指定方法、指定位置上的编程思想就是面向切面的编程(AOP)
为什么需要AOP?
  + 如果想在某些方法前后添加一种功能，那么AOP就显得很有用了.
  + 例子:
    ```java
    /*
    我们想要在方法调用添加一些功能：
        1. 打印日志
        2. 鉴权
        3. 缓存
    */
    // spring使用切面，是通过字节码增强，生成Aservice子类, Aservice$$
    class AService {
            a() {}
            b() {}
            c() {}
            // ...
    }

    // 切面所指定的方法
    class Aspect {
      log() {
        // 每当由xx方法调用时，自动调用log方法
      }
      cache() {
        // 每当由xx方法调用时，自动调用cache方法
      }
    }
      ``` 

## Bean 的声明周期
+ 完成服务功能的对象称为Bean
+ Spring维护了一套完整的生命周期，每个Bean都可以自定义在生命周期任何一个阶段所完成的工作
+ 实例化 -> 填充属性 -> 调用BeanNameAware的setBeanName方法 -> 调用BeanFactoryAware的setBeanFactory方法
->调用ApplicationContextAware的setApplicationContext方法 -> 调用BeanPostprocess的postProcessBeforeInitialization
-> 调用ionitializingBean的afterPropertiesSet方法 -> 调用定制的初始化方法 -> 调用BeanPostProcess的
postProcessAfterinitialization-> Bean准备就绪->调用DispostbleBean的destory方法->调用定制的销毁方法
+ bean resource load(BeanDefinitionReader)
+ Bean从零开始: 从指定的配置文件中加载BeanDefinition,然后Spring容器根据该定义开始进行Bean的装配工作

## Spring Boot 相当于Spring MVC有哪些改进?
+ 自动化配置: 
  + Spring MVC - 通过XML配置，手工指定
  + Spring Boot - 通过注解或者自动化扫描
+ 自带Servlet容器， 可以直接启动
  + Spring MVC - 不自带Servlet容器， 需要额外配置
  + Spring Boot - 直接打成jar包， 可以直接启动
+ Spring Boot 对于微服务和快速开发更为友好
  + 优点: 大大简化了开发配置的难度
  + 缺点: 有很多潜在的默认约定，使开发者更难深入了解其中的细节