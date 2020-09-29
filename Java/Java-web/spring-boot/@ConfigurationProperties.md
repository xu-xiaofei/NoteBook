
1. @ConfigurationProperties
    此注解需要和@Component 之类的注解搭配再一起使用，因为只有注入到IOC容器的类才有被ConfigurationProperties赋值的意义。

    ```java

    @RestController
    public class TestEnableConfigurationProperties {

        @Data
        @ConfigurationProperties(prefix = "server")
        @Component
        public static class HelloServiceProperties {

            private String port;
        }

        @Autowired
        private HelloServiceProperties helloServiceProperties;

        @RequestMapping("/getObjectProperties")
        public Object getObjectProperties() {
            System.out.println(helloServiceProperties.getPort());
            return helloServiceProperties.getPort();
        }
    }
    ```


    2. ConfigurationProperties 如果没有和Component一起使用，则需要通过EnableConfigurationProperties来进行实例化

    ```java
    @RestController
    public class TestEnableConfigurationProperties {

        @ConfigurationProperties(prefix = "server")
        @Data
        public static class HelloServiceProperties {

            private String port;
        }

        @EnableConfigurationProperties(HelloServiceProperties.class)
        public static class HelloServiceAutoConfiguration {

        }

        @Resource
        private HelloServiceProperties helloServiceProperties;

        @RequestMapping("/getObjectProperties")
        public Object getObjectProperties() {
            System.out.println(helloServiceProperties.getPort());
            return helloServiceProperties.getPort();
        }
    }

    ```