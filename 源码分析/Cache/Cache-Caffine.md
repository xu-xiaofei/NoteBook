1. Caffine 和Spring boot 整合

 - 核心类有两个 org.springframework.cache.Cache 和 org.springframework.cache.CacheManager

 - Cache 定义了缓存的存取操作的

 - CacheManager 定义了缓存的管理最大容量和过期设置

 2. Caffine 实现了相应的接口，使用Cacheable 注解来使用即可

 示例关键代码
 ```java
import com.github.benmanes.caffeine.cache.Caffeine;
import org.springframework.cache.CacheManager;
import org.springframework.cache.caffeine.CaffeineCacheManager;
import org.springframework.cache.interceptor.KeyGenerator;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.Arrays;
import java.util.concurrent.TimeUnit;

/**
 * @Author fei
 */
@Configuration
public class CacheConf {

    public static final String MY_KEY = "KeyGenerateBean";

    @Bean
    public CacheManager initCacheManager() {
        CaffeineCacheManager caffeineCacheManager = new CaffeineCacheManager();
        caffeineCacheManager.setCaffeine(Caffeine.newBuilder()
                .initialCapacity(16)
                .maximumSize(10_1000)
                .expireAfterWrite(10, TimeUnit.SECONDS));
        return caffeineCacheManager;
    }

    @Bean(MY_KEY)
    public KeyGenerator initKeyGenerate() {
        KeyGenerator generator = (target, method, param) -> generate(target.getClass().getName(), method.getName(), param);
        return generator;
    }

    private Object generate(Object... param) {
        return Arrays.deepHashCode(param);
    }


}



 ````
