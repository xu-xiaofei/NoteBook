## Redis 的订阅发布模式
    订阅者需要继承JedisPubSub，通过onMessage 来实现自己的功能，farmerJedis.subscribe(farmer,CHANNEL_NAME)  
    发布者则可以直接 jedis.publish(channel,msg);
    

```java
import lombok.extern.slf4j.Slf4j;
import org.slf4j.Logger;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;

import javax.annotation.Resource;

/**
 * @author: xiaofei.xu
 **/
@Slf4j
@Controller
public class TestPublisher {

    public static final String CHANNEL_NAME = "MyChannel";

    private final static Logger logger =log;

    @Resource
    private JedisPool JEDIS_POOL;

    @GetMapping("/redis/publish")
    public void redisPublish() throws Exception {

        /*订阅者redis客户端*/
        final Jedis farmerJedis = JEDIS_POOL.getResource();
        final Jedis workerJedis = JEDIS_POOL.getResource();
        final Jedis programmerJedis = JEDIS_POOL.getResource();

        /*发布者redis*/
        final Jedis publisherJedis = JEDIS_POOL.getResource();

        final Farmer farmer = new Farmer();
        final Worker worker = new Worker();
        final Programmer programmer  = new Programmer();

        //订阅线程：接收消息，因为Jedis是以阻塞的方式等待发布者消息的，所以每个Jedis客户端必须对应一个独立的线程。不然只会有第一个Jedis接受到消息。
        new Thread(new Runnable() {
            public void run() {
                try {
                    farmerJedis.subscribe(farmer,CHANNEL_NAME);
                    logger.info("Subscription ended.");
                } catch (Exception e) {
                    logger.error("Subscribing failed.", e);
                }
            }
        }).start();

        new Thread(new Runnable() {
            public void run() {
                try {
                    workerJedis.subscribe(worker,CHANNEL_NAME);
                    logger.info("Subscription ended.");
                } catch (Exception e) {
                    logger.error("Subscribing failed.", e);
                }
            }
        }).start();

        new Thread(new Runnable() {
            public void run() {
                try {
                    programmerJedis.subscribe(programmer,CHANNEL_NAME);
                    logger.info("Subscription ended.");
                } catch (Exception e) {
                    logger.error("Subscribing failed.", e);
                }
            }
        }).start();

        //主线程：发布消息到CHANNEL_NAME频道上
        WeatherServer weatherServer = new WeatherServer();
        weatherServer.publishMessage(publisherJedis,CHANNEL_NAME,"rain");

        Thread.sleep(2000);
        weatherServer.publishMessage(publisherJedis,CHANNEL_NAME,"Sunny");

        farmerJedis.close();
        workerJedis.close();
        programmerJedis.close();
    }

}
```

### WetherServer

```java
package com.envisioniot.trc.backendservice.test.redis;

import redis.clients.jedis.Jedis;

import javax.annotation.Resource;

/**
 * @author: xiaofei.xu
 **/

public class WeatherServer {

    public void publishMessage(Jedis jedis, String channel, String msg) {
        jedis.publish(channel,msg);
    }
}
```

### Programmer

```java

import lombok.extern.slf4j.Slf4j;
import org.slf4j.Logger;
import redis.clients.jedis.JedisPubSub;

/**
 * @author: xiaofei.xu
 **/
@Slf4j
public class Programmer extends JedisPubSub {

    private static final Logger logger = log;

    @Override
    public void onMessage(String channel, String message) {
        logger.info(String.format("client: PROGRAMMER. Message. Channel: %s, Msg: %s", channel, message));
        if("rain".equals(message)){
            logger.info("PROGRAMMER : a satisfied day!!!");
        }else if("sunny".equals(message)){
            logger.info("PROGRAMMER : a terrible day!!!");
        }else {
            logger.info("PROGRAMMER : Spam messages");
        }
    }

}
```