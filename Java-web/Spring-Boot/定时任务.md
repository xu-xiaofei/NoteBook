###定时器

1. Timer


2. @Schedule注解

    + 2.1 第一步在Application 启动类上开启注解@EnableScheduling
    + 2.2 第二步在任意可以被扫描到的Bean内部的method上，加上注解@Scheduled
```java
@Component
public class TestSchedule(){

    @Scheduled(fixedDelay=10)
    public void testTask(){
        System.out.println("This is a task")
    }
}
```


3. Quartz框架

一个jobDetail可以被多个trigger触发，所以是一对多的关系
一个schedule可以被多个触发器使用，所以是一对多的关系??


