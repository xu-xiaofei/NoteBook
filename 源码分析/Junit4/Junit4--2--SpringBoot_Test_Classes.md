### @RunWith(Suite.class)分析

例子:
```java
@RunWith(Suite.class)
@Suite.SuiteClasses({
    TestEnvironmentController.class,
    //More classes to add for testing
    })
public class EntryTest {
    //Do nothing
}
```

Spring boot 的测试框架的搭建,针对于controller 层的入口，要测试controller层，则其子类继承BaseApplicationTest即可
```java
@AutoConfigureMockMvc
@ExtendWith(SpringExtension.class)
@RunWith(SpringRunner.class)
@SpringBootTest(classes = BackendServiceApplication.class, webEnvironment = SpringBootTest.WebEnvironment.MOCK)
public class BaseApplicationTest {
    //基类

    @Resource
    protected MockMvc mockMvc;

}

子类实例

@Slf4j
public class TestEnvironmentController extends BaseApplicationTest {

    private String baseUrl = "/trc/environment";

    @Test
    public void testGetBranches() throws Exception {
        MvcResult mvcResult = mockMvc.perform(
                get(baseUrl + "/branches")
        ).andReturn();

        log.info("The post facility result is {}",JSON.toJSONString(mvcResult.getResponse()));
    }
}

```

