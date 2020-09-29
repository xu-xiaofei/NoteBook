### Springboot 集成mybatis 完成分页插件
1. 引入POM  pageHlper的github地址 [地址](https://github.com/pagehelper/pagehelper-spring-boot)

```
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper-spring-boot-starter</artifactId>
			<version>1.2.12</version>
		</dependency>
```     

2. 使用Demo
```java
import javax.annotation.Resource;
import java.util.List;

/**
 * @author: xiaofei.xu
 **/
@RestController
public class PageController {

    @Resource
    private QueryHistoryMapper queryHistoryMapper;

    @GetMapping("/getAllHistory")
    public String getAllHistory(Model model, @RequestParam(defaultValue = "1", value = "pageNum") Integer pageNum) {
        PageHelper.startPage(pageNum, 2);

        List<LotQueryHistory> histories = queryHistoryMapper.getAllHistoriesTest();
        PageInfo<LotQueryHistory> pageInfo = new PageInfo<>(histories);
        model.addAttribute("pageInfo", pageInfo);
        return "list";
    }
}
```


3.  分析

pageHelper 执行了两个SQL，显示查询了count，第二句才去进行了limit  

![pageHelper执行的info](/Pictures/PageHelper.png)


4. 其他的分页方法

    - subList
    - 手动计算limit
    - 拦截器为sql 添加limit  
  [参考](https://juejin.im/entry/59127ad4da2f6000536f64d8)*
  