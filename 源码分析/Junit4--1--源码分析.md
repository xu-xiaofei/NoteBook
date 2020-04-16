### Junit4 源码分析

1. Junit4运行前的构建过程

2. Junit4的入口类

3. Junit4的运行过程
```java
    //Statement 是要执行的方法的集合
    protected Statement methodBlock(FrameworkMethod method) {
        Object test =反射构造一个测试的实例类

        Statement statement = methodInvoker(method, test);
        //检查是否配置了注解，例如@Test
        statement = possiblyExpectingExceptions(method, test, statement);
        //读取@Test的时间，默认为0
        statement = withPotentialTimeout(method, test, statement);
        
        statement = withBefores(method, test, statement);
        statement = withAfters(method, test, statement);

        //Rule分两种，TestRule 和MethodRule    
        statement = withRules(method, test, statement);
        return statement;
    }
```
4. Junit4的通知机制

5. Junit4的常用注解

6. Junit4的Rule

