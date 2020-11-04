
1. 不生效的@Async

```java
    /**
     * 1.同一个类中的方法调用@Async 修饰的方法是不生效的，因为无法不是调用的代理类
     * 所以使用了内部类的方法来实现一部方法
     * 2. 必须使得@Async注解修饰的类或者方法被注入到Spring管理容器
     */
    @Async
    @Component
    public static class BusSubsidyReportUtil {
        
    }
```

2. @Async加载接口或者接口方法上子类实现依然生效
```java
public interface BusSubsidyReportService extends BaseService<BusSubsidyReport> {

    @Async
    void generateReport(Long stationId, Date startDate, Date endDate);

}
```