### 在Springboot 中使用cache

1. 直接使用，示例使用代码
在controller层的方法上面直接使用 ` @Cacheable(cacheNames = "getAllChainLotStruct", key = "#lot")`  
cacheNames 是CacheManager里面维持的cacheMap的key,key 是cache里面的key,cache里面的value是方法的返回值  
key 如果不指定就是方法的所有参数  
`private final ConcurrentMap<String, Cache> cacheMap = new ConcurrentHashMap<>(16);`

```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

/**
 * @author: xiaofei.xu
 **/
@RestController
@Slf4j
public class TestCacheController {

    @GetMapping("/cache/test/lot1")
    @Cacheable(cacheNames = "getAllChainLotStruct", key = "#lot")
    public String getAllChainLotStruct(@RequestParam String lot) {
        log.info("It has been go here really {}", lot);
        return lot + " __1__ ";
    }

    @GetMapping("/cache/test/lot2")
    @Cacheable(cacheNames = "getAllChainProcess", key = "#lot")
    public String getAllChainProcess(@RequestParam String lot) {
        log.info("It has been go here really {}", lot);
        return lot + "__2__ ";
    }
}

```

2. 配置CacheManager  
```java
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.cache.caffeine.CaffeineCacheManager;
import org.springframework.cache.concurrent.ConcurrentMapCacheManager;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;

/**
 * @author: xiaofei.xu
 **/

@Configuration
@EnableCaching
public class CacheConfig {

    public interface CacheManagerName {
        /**
         * Redis
         */
        String REDIS_CACHE_MANAGER = "redisCacheManager";

        /**
         * Concurrent Map
         */
        String MAP_CACHE_MANAGER = "mapCacheManager";

        String CAFFEINE_CACHE_MANAGER = "caffeineCacheManager";
    }

    @Bean(CacheManagerName.REDIS_CACHE_MANAGER)
    public RedisCacheManager createRedisCacheManager(RedisConnectionFactory connectionFactory) {
        return RedisCacheManager.create(connectionFactory);
    }


    @Bean(CacheManagerName.MAP_CACHE_MANAGER)
    public ConcurrentMapCacheManager createMapCacheManager() {
        ConcurrentMapCacheManager concurrentMapCacheManager = new ConcurrentMapCacheManager();
        return concurrentMapCacheManager;
    }

    @Primary  //The default cache manager
    @Bean(CacheManagerName.CAFFEINE_CACHE_MANAGER)
    public CaffeineCacheManager createCaffeineCacheManager() {
        CaffeineCacheManager caffeineCacheManager = new CaffeineCacheManager();
        return caffeineCacheManager;
    }

}

```
CacheManager 规定了一系列的cache 的操作put,set，get等等

3. 默认配置

如果yml文件里面配置了redis，那么在不指定cacheManager的情况下，springboot会默认使用redis作为缓存
Springboot 的默认缓存是ConcurrentMapCacheManager，是一个并发map做的缓存

