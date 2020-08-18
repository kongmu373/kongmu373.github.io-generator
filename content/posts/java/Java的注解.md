---
title: "Java的注解"
date: 2020-06-10T16:21:49+08:00
draft: true
tags: ["注解"]
series: ["Java体系"]
categories: ["Java体系"]
---


## 什么是注解?
+ Class是什么？
  + Class是Java类的说明书
  + 你或者JVM阅读该说明书，创建类的实例
+ 注解就是说明书中的一小段信息/文本/标记
  + Annotation
  + 可以携带参数
  + 可以在运行时被阅读

## 注解怎么写?
+ 新建一个类选择注解
+ 元注解
  + @Rentention 保留,RetentionPolicy 默认CLASS 2
  + @Target ElementType
  + @Documented
  + @Inherited
  + @Repeatable

## 注解的属性
+ 可以有哪些?
  + 基本数据类型+String+类以及它们的数组
  + 默认值
  + 名为value的属性

## JDK的自带注解
+ @Deprecated
+ @Override
+ @SuppressWarnings
+ @FunctionalInterface

## 注解是如何工作的?
+ 注解仅仅是一段信息，它自己无法工作
```java
public class MyService {
    @Log
    public void queryDatabase(int param) {
        System.out.println("query db:" + param);
       
    }
}

public @interface Log {

}

// 处理注解
// 动态字节码增强(修改说明书,提供原先不存在的功能)
// byte-buddy
public static void main(String[] args) {   
  MyService service = enhanceByAnnotation();
  service.queryDatabase(1);
}
private static MyService enhanceByAnnotation() {
    return null;
}

```
## 通过反射获取注解
+ Method.getAnnotation
+ Class.getAnnotation

## 基于注解的日志实现
+ @Log注解
+ 在运行时，拦截方法的进入和推出，打印相应的日志

## 基于注解的缓存实现
+ @Cache注解
+ 如果缓存中不存在方法调用的结果
  + 调用真实的方法
  + 将结果放入缓存
+ 如果缓存中以及存在结果，检查是否过期，如果过期
  + 调用真实的方法
  + 将结果放入缓存
+ 否则 ，缓存中存在结果且不过期
  + 直接返回缓存中的结果

```java
//Cache.java
@Retention(RetentionPolicy.RUNTIME)
public @interface Cache {
    // 标记缓存的时长（秒），默认60s
    int cacheSeconds() default 60;
}

// CacheAdvisor.java
public class CacheAdvisor {

    private static class CacheKey {
        private Method method;
        private Object thisObject;
        private Object[] arguments;

        CacheKey(Method method, Object thisObject, Object[] arguments) {
            this.method = method;
            this.thisObject = thisObject;
            this.arguments = arguments;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) {
                return true;
            }
            if (o == null || getClass() != o.getClass()) {
                return false;
            }
            CacheKey cacheKey = (CacheKey) o;
            return Objects.equals(method, cacheKey.method) &&
                    Objects.equals(thisObject, cacheKey.thisObject) &&
                    Arrays.equals(arguments, cacheKey.arguments);
        }

        @Override
        public int hashCode() {
            int result = Objects.hash(method, thisObject);
            result = 31 * result + Arrays.hashCode(arguments);
            return result;
        }
    }

    private static class CacheValue {
        private Object value;
        private Long time;

        CacheValue(Object value, Long time) {
            this.value = value;
            this.time = time;
        }
    }

    private static ConcurrentHashMap<CacheKey, CacheValue> map = new ConcurrentHashMap<>();

    @RuntimeType
    public static Object cache(@SuperCall Callable<Object> zuper,
                               @Origin Method method,
                               @This Object thisObject,
                               @AllArguments Object[] arguments
    ) throws Exception {
        CacheKey cacheKey = new CacheKey(method, thisObject, arguments);

        CacheValue cacheValue = map.get(cacheKey);

        if (cacheValue == null || isCacheKeyExpired(cacheValue, method)) {
            cacheValue = putValueIntoMapAndReturnCacheValue(cacheKey, zuper);
        }

        return cacheValue.value;
    }

    private static boolean isCacheKeyExpired(CacheValue cacheValue, Method method) {
        return (System.currentTimeMillis() - cacheValue.time) / 1000 > method.getAnnotation(Cache.class).cacheSeconds();
    }

    private static CacheValue putValueIntoMapAndReturnCacheValue(CacheKey cacheKey, Callable<Object> zuper) throws Exception {
        CacheValue value = new CacheValue(zuper.call(), System.currentTimeMillis());
        map.put(cacheKey, value);
        return value;
    }
}


// CacheClassDecorator.java
public class CacheClassDecorator {
    // 将传入的服务类Class进行增强
    // 使得返回一个具有如下功能的Class：
    // 如果某个方法标注了@Cache注解，则返回值能够被自动缓存注解所指定的时长
    // 这意味着，在短时间内调用同一个服务的同一个@Cache方法两次
    // 它实际上只被调用一次，第二次的结果直接从缓存中获取
    // 注意，缓存的实现需要是线程安全的
    @SuppressWarnings("unchecked")
    public static <T> Class<T> decorate(Class<T> klass) {
        return (Class<T>) new ByteBuddy()
                .subclass(klass)
                .method(ElementMatchers.isAnnotatedWith(Cache.class))
                .intercept(MethodDelegation.to(CacheAdvisor.class))
                .make()
                .load(klass.getClassLoader())
                .getLoaded();
    }

    public static void main(String[] args) throws Exception {
        DataService dataService = decorate(DataService.class).getConstructor().newInstance();

        // 有缓存的查询：只有第一次执行了真正的查询操作，第二次从缓存中获取
        System.out.println(dataService.queryData(1));
        Thread.sleep(1 * 1000);
        System.out.println(dataService.queryData(1));

        // 无缓存的查询：两次都执行了真正的查询操作
        System.out.println(dataService.queryDataWithoutCache(1));
        Thread.sleep(1 * 1000);
        System.out.println(dataService.queryDataWithoutCache(1));
    }
}

// DataService.java
public class DataService {
    /**
     * 根据数据ID查询一列数据，有缓存。
     *
     * @param id 数据ID
     * @return 查询到的数据列表
     */
    @Cache
    public List<Object> queryData(int id) {
        // 模拟一个查询操作
        Random random = new Random();
        int size = random.nextInt(10) + 10;
        return IntStream.range(0, size)
                .mapToObj(i -> random.nextInt(10))
                .collect(Collectors.toList());
    }

    /**
     * 根据数据ID查询一列数据，无缓存。
     *
     * @param id 数据ID
     * @return 查询到的数据列表
     */
    public List<Object> queryDataWithoutCache(int id) {
        // 模拟一个查询操作
        Random random = new Random();
        int size = random.nextInt(10) + 1;
        return IntStream.range(0, size)
                .mapToObj(i -> random.nextBoolean())
                .collect(Collectors.toList());
    }
}
```



