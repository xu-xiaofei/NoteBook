## 1. 引入依赖
```java

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-websocket</artifactId>
		</dependency>
```

##　2. 代码配置(否则自动断开)
```java

@Configuration
public class WebSocketConfig {
    /**
     * 这个bean会自动注册使用@ServerEndpoint注解声明的websocket endpoint
     * @return
     */
    @Bean
    public ServerEndpointExporter serverEndpointExporter(){
        return  new ServerEndpointExporter();
    }
}
```

## 3. 接入点配置
```java
@Slf4j
@ServerEndpoint(value = "/ws/svg")
@Component
public class WebSocketServer {

	@PostConstruct
	public void post(){
		System.out.println("post");
	}

	// 连接
	@OnOpen
	public void onOpen(Session session) {
		System.out.println("open");

	}

	// 关闭
	@OnClose
	public void onClose(Session session) {
		System.out.println("close");
	}

	// 接收消息 客户端发送过来的消息
	@OnMessage
	public void onMessage(String message, Session session) {
		System.out.println("message");
	}

	// 异常
	@OnError
	public void onError(Session session, Throwable throwable) {
		log.error("##### webscoket 异常：{}",throwable.getMessage());
	}


}

```

## 4. spring gateway 配置（否则自动断开）

```java
  {
    "routeId": "sesp-scada-intranet-xiaofei-ws",
    "routeName": "SCADA-内网-ws",
    "predicates": [
      {
        "args": {
          "_genkey_0": "/sesp-scada-intranet-xiaofei/ws/**"
        },
        "name": "Path"
      }
    ],
    "filters": [],
    "uri": "lb:ws//sesp-scada-intranet-xiaofei",
    "seq": 0,
    "createTime": null,
    "updateTime": null,
    "delFlag": "0"
  },

```

## 5. nacos 配置（否则认证不过，自动断开）
```yml
security:
  oauth2:
    client:
      client-id: sesp-scada-intranet
      client-secret: sesp-scada-intranet
      scope: server
      ignore-urls:
        - /druid/**
        - /actuator/**
        - /v2/api-docs
        - /ws/**

```

