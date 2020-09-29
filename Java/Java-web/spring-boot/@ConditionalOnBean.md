

@ConditionalOnBean   创建Bean的时候，指定的Bean得存在
@ConditionalOnClass  创建Bean的时候，指定的Class得存在

@ConditionalMissingBean   创建Bean的时候，指定的Bean不能存在
@ConditionalMissingClass  创建Bean的时候，指定的Class不能存在

```
org.springframework.boot.autoconfigure.context.condition
ConditionalOnCloudPlatform：是否在云环境下，spring boot cloud模块提供了两种实现，CLOUD_FOUNDRY和HEROKU，国内应该用不到这个注解了
ConditionalOnJava：指定的Java版本
ConditionalOnWebApplication：是Web环境的时候
ConditionalOnNotWebApplication：不是web环境的时候
ConditionalOnJndi：JNDI环境下使用
ConditionalOnClass：classpath中存在某个类
ConditionalOnMissingClass：classpath中不存在某个类
ConditionalOnBean：BeanFactory中存在某个类的Bean
ConditionalOnMissingBean：BeanFactory中不存在某个类的Bean
ConditionalOnExpression：SpEL的结果
ConditionalOnProperty：Environment中是否有某个属性的配置信息
ConditionalOnResource：classpath中是否存在指定名称的资源
ConditionalOnSingleCandidate：指定的类在BeanFactory中只有一个候选的bean，或者有多个候选的bean，但是其中一个指定了primary时
各种*AutoConfiguration的实现：
所有的*AutoConfiguration的具体实现包括两部分，一个是标识了@Configuration注解的配置类，另一个是Property文件。有些模块比较复杂，像security的oauth2模块，主要文件也是这两类，剩下的是一些工具。

*AutoConfiguration也是Configuration，被@Configuration注解，只不过spring boot autoconfigure模块内置的 *AutoConfiguration被配置到了 spring.factories文件中，启动的时候自动配置。


自动配置是Spring Boot的最大亮点，完美的展示了IoC约定由于配置。Spring Boot能自动配置Spring各种子项目（Spring MVC, Spring Security, Spring Data, Spring Cloud, Spring Integration, Spring Batch等）以及第三方开源框架所需要定义的各种Bean。 
Spring Boot内部定义了各种各样的XxxxAutoConfiguration配置类，预先定义好了各种所需的Bean。只有在特定的情况下这些配置类才会被起效。 
```

