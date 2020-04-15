### @RunWith(Suite.class)分析

例子:
疑问：使用文件配置的话怎么做？
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({SuperTest.class})
public class EntryTest {
}
```