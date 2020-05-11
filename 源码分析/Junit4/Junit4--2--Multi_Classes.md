### @RunWith(Suite.class)分析

例子:
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({
    SuperTest.class,
    //More classes to add for testing
    })
public class EntryTest {
    //Do nothing
}
```

