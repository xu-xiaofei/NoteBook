### WebSocket

### 1. STOMP 协议
    STOMP即Simple (or Streaming) Text Orientated Messaging Protocol，简单(流)文本定向消息协议，它提供了一个可互操作的连接格式，允许STOMP客户端与任意STOMP消息代理（Broker）进行交互。STOMP协议由于设计简单，易于开发客户端，因此在多种语言和多种平台上得到广泛地应用。

    STOMP协议的前身是TTMP协议（一个简单的基于文本的协议），专为消息中间件设计。

    STOMP是一个非常简单和容易实现的协议，其设计灵感源自于HTTP的简单性。尽管STOMP协议在服务器端的实现可能有一定的难度，但客户端的实现却很容易。例如，可以使用Telnet登录到任何的STOMP代理，并与STOMP代理进行交互。

### 2. 异步消息代理／JMS
    JMS即Java消息服务（Java Message Service）应用程序接口是一个Java平台中关于面向消息中间件（MOM）的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。

    JMS是一种与厂商无关的API，用来访问消息收发系统消息。类似于JDBC , slf4j 等等API,十亿桶通用的接口,任何人或者厂商都可以实现这套API,编写自己的消息中间件

+ 常用的JMS实现
    
    要使用Java消息服务，你必须要有一个JMS提供者，管理会话和队列。既有开源的提供者也有专有的提供者。

    Apache ActiveMQ , OpenJMS

+ JMS由以下元素组成。  
    JMS提供者provider：连接面向消息中间件的，JMS接口的一个实现。提供者可以是Java平台的JMS实现，也可以是非Java平台的面向消息中间件的适配器。  
    JMS客户：生产或消费基于消息的Java的应用程序或对象。  
    JMS生产者：创建并发送消息的JMS客户。  
    JMS消费者：接收消息的JMS客户。  
    JMS消息：包括可以在JMS客户之间传递的数据的对象  
    JMS队列：一个容纳那些被发送的等待阅读的消息的区域。与队列名字所暗示的意思不同，消息的接受顺序并不一定要与消息的发送顺序相同。一旦一个消息被阅读，该消息将被从队列中移走。  
    JMS主题：一种支持发送消息给多个订阅者的机制  

### 3. spring-messaging
    websocket的以来中包含了对messaging的依赖,也是一种异步消息中间级的实现,但是我还不确定是不是JMS的实现


   





