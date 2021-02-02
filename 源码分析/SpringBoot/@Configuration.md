### @Configuration

    @Configuration(proxyBeanMethods=false)

1. True  seviceA and serviceB 使用的是同一个serviceC实例，也就是serviceC只会new 一次

```java
@Configuration(proxyBeanMethods=true)
public class SomeConfiguration {
    @Bean
    ServiceA serviceA(){
      return new ServiceA(sharedService());
    }

    @Bean
    ServiceB serviceB(){
      return new ServiceB(sharedService());
    }

    @Bean
    ServiceC sharedService(){
      return new ServiceC();
    }
}
```

2. False  seviceA and serviceB 使用的不是同一个serviceC实例

```java
@Configuration(proxyBeanMethods=false)
public class SomeConfiguration {
    @Bean
    ServiceA serviceA(){
      return new ServiceA(sharedService());
    }

    @Bean
    ServiceB serviceB(){
      return new ServiceB(sharedService());
    }

    @Bean
    ServiceC sharedService(){
      return new ServiceC();
    }
}

```




