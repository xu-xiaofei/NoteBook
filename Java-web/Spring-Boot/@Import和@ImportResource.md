
### @Import
    @Import注解是引入java类：

    导入@Configuration注解的配置类（4.2版本之前只可以导入配置类，4.2版本之后也可以导入普通类）
    导入ImportSelector的实现类
    导入ImportBeanDefinitionRegistrar的实现类

    @Import 是被用来整合所有在@Configuration注解中定义的bean配置。这其实很像我们将多个XML配置文件导入到单个文件的情形。

    当写的配置文件不在APplication下面的包的时候，可以通过Import来实现将Bean注入到容器中
    这就是第三方组件使用EnableXXX的原因

    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    @Documented
    @Import(ParentConfig.class)
    public @interface TestMyAnno {
    }
    ```

[详细参考](https://segmentfault.com/a/1190000011068471)

### @ImportResource

    @ImportResource是引入spring配置文件.xml
    `@ImportResource("classpath:cons-injec.xml")`