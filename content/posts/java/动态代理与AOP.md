---
title: "动态代理与AOP"
date: 2020-06-15T16:08:15+08:00
draft: true
tags: ["Spring"]
series: ["Spring"]
categories: ["Spring"]
---

## 什么是AOP
+ Aspect-Oriented Programming 面向切面编程
+ 相对于OOP (面向对象编程)
+ AOP是面向切面编程，关注一个统一的切面
+ AOP和Spring是不同的东西

## AOP适合于哪些场景
+ 需要统一处理的场景
  + 日志
  + 缓存
  + 鉴权
+ 如果用OOP来做需要怎么办?
  + 装饰器模式

## 装饰器模式
+ Decorator pattern
+ 动态地为一个对象增加功能，但是不改变其结构
+ 本质上是一个"包装"
+ 装饰器模式的实现
```java
// 首先，需要装饰的方法
//DataService.java
public interface DataService {
    String a(int i);

    String b(int i);
}
// DataServiceImpl
public class DataServiceImpl implements DataService{

    @Override
    public String a(int i) {
        return String.valueOf(UUID.randomUUID());
    }

    @Override
    public String b(int i) {
        return String.valueOf(UUID.randomUUID());
    }
}
// 针对DataService每个方法之前都打印日志
// LogDecorator.java
public class LogDecorator implements DataService{

    private DataService delegate;

    public LogDecorator(DataService delegate) {
        this.delegate = delegate;
    }

    @Override
    public String a(int i) {
        System.out.println("打印: " + i);
        return delegate.a(i);
    }

    @Override
    public String b(int i) {
        System.out.println("打印: " + i);
        return delegate.b(i);
    }
}
// CacheDecorator.java
public class CacheDecorator implements DataService {

    private Map<Integer, String> cache = new HashMap<>();

    private DataService delegate;

    public CacheDecorator(DataService delegate) {
        this.delegate = delegate;
    }

    @Override
    public String a(int i) {
        String s = cache.get(i);
        if (s == null) {
            s = delegate.a(i);
            cache.put(i, s);
        }
        return s;
    }

    @Override
    public String b(int i) {
        String s = cache.get(i);
        if (s == null) {
            s = delegate.a(i);
            cache.put(i, s);
        }
        return s;
    }
}
// 输出 Main.java
public class Main {
    private static DataService dataService = new CacheDecorator(new LogDecorator(new DataServiceImpl()));

    public static void main(String[] args) {
        System.out.println(dataService.a(1));
        System.out.println(dataService.b(2));
        System.out.println(dataService.a(1));
    }
}

```
## AOP的实现
+ JDK动态代理
+ 优点: 不需要依赖任何第三方库
+ 缺点: 功能受限，只适用于接口
```java
// LogProxy.java 
public class LogProxy implements InvocationHandler {
    private DataService dataService;

    public LogProxy(DataService dataService) {
        this.dataService = dataService;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(method.getReturnType() + ", args: " + Arrays.toString(args));
        Object reValue = method.invoke(dataService, args);
        System.out.println("reValue: " + reValue);
        return reValue;
    }
}
// 输出 Main.java
public class Main {
    private static DataService dataService = new DataServiceImpl();

    public static void main(String[] args) {
        dataService = (DataService) Proxy.newProxyInstance(dataService.getClass().getClassLoader(),
                new Class[]{DataService.class}, new LogProxy(dataService));
        System.out.println(dataService.a(1));
        System.out.println(dataService.b(1));
    }
}
```
+ CGLIB/ByteBuddy字节码生成
+ 优点: 强大, 不受接口的限制
+ 缺点: 需要引用额外的第三方类库
  + 不能增强final类/final/private 方法
```java
// 用CGLIB实现类的动态代理
public class LogCglib implements MethodInterceptor {
    private DataService dataService;

    public LogCglib(DataService dataService) {
        this.dataService = dataService;
    }

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        System.out.println(method.getReturnType() + ", args: " + Arrays.toString(objects));
        Object reValue = method.invoke(dataService, objects);
        System.out.println("reValue: " + reValue);
        return reValue;
    }
}

public class Main {
    private static DataService dataService = new DataServiceImpl();


    public static void main(String[] args) {

        System.out.println(dataService.a(1));
        System.out.println(dataService.b(1));
    }
}

// 用byteBuddy第三方库实现动态代理
    private  MyService enhanceByAnnotation(Class<MyService> myServiceClass) throws IllegalAccessException, InstantiationException {
        return new ByteBuddy()
                .subclass(myServiceClass)
                .method(isAnnotatedWith(Log.class)).intercept(MethodDelegation.to(LoggerInterceptor.class))
                .make()
                .load(Main.class.getClassLoader())
                .getLoaded()
                .newInstance();
    }

    public class LoggerInterceptor {
        public static void log(@SuperCall Callable<Void> zuper)
                throws Exception {
            System.out.println("Calling database");
            try {
                zuper.call();
            } finally {
                System.out.println("Returned from database");
            }
        }
    }
        public static void main(String[] args) throws InstantiationException, IllegalAccessException {
       MyService myService = enhanceByAnnotation(MyService.class);
       myService.queryDatabase(1);

    }
```
## AOP与Spring
+ 在Spring中使用AOP实现Redis缓存
+ Spring是如何切换JDK动态代理和CGLIB的?
  + spring.aop.proxy-target-class=true
+ @Aspect声明切面
  + @Before
  + @After
  + @Around

## 什么是Redis
+ 广泛使用的内存缓存
+ 常见的数据结构
  + String/List/Set/Hash/ZSet
+ Redis 为什么这么快
  + 完全基于内存
  + 优秀的数据结构设计
  + 单一线程，避免上下文切换开销
  + 事件驱动，非阻塞

```java
@Configuration
public class Appconfig {

    @Bean
    RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        return template;
    }
}
@Retention(RetentionPolicy.RUNTIME)
public @interface Cache {
}
@Aspect
@Configuration
public class CacheAspect {
    @Autowired
    private RedisTemplate redisTemplate;

    @Around("@annotation(kongmu373.config.Cache)")
    public Object cache(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        Signature signature = proceedingJoinPoint.getSignature();
        String name = signature.getName();
        Object cachedValue = redisTemplate.opsForValue().get(name);
        if(cachedValue == null) {
            System.out.println("Get value from database!");
            cachedValue = proceedingJoinPoint.proceed();
            redisTemplate.opsForValue().set(name, cachedValue);
        } else {
            System.out.println("Get value from cache");
        }

        return cachedValue;
    }
}
@Component
public class RankDao {
    @Autowired
    private SqlSession sqlSession;


    @Cache
    public List<Rank> selectRankItemList() {
        return sqlSession.selectList("MyMapper.selectRankItemList");
    }
}

```


