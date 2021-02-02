在开发基于 Spring 的应用程序时，我们可能需条件性地注册 Bean。

例如，你希望在本地运行应用程序时注册一个指向 DEV 数据库的 DataSource bean，在生产中指向 PRODUCTION 数据库。

你可以将数据库连接参数外部化，放置在属性（properties）文件中，并在适当的环境中使用该个文件。但是，当你需要指向不同的环境并构建应用程序时，你得更改配置。

为了解决这个问题，Spring 3.1 引入了 Profiles 概念。你可以注册多个相同类型的 bean，并将它们与一个或多个配置文件相关联。在运行应用程序时，你可以激活所需的配置文件，只有与配置文件相关联的 bean 才被注册。

```java
@Configuration
public class AppConfig
{
    @Bean
    @Profile("DEV")
    public DataSource devDataSource() {
        ...
    }
 
    @Bean
    @Profile("PROD")
    public DataSource prodDataSource() {
        ...
    }
}
```

然后，可以使用系统属性（System Property）来指定要激活配置文件 - Dspring.profiles.active = DEV