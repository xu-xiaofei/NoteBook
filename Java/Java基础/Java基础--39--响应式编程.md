### 响应式编程

前置的知识点：函数式编程，Stream流，lamda表达式

1. POM.xml
```
        <dependency>
            <groupId>io.reactivex.rxjava2</groupId>
            <artifactId>rxjava</artifactId>
            <version>2.2.19</version>
        </dependency>

```

2. Demo  __Observable__

```java
    public void  testObservable(){
        ConnectableObservable<String> source =
                Observable.just("Alpha","Beta","Gamma","Delta","Epsilon")
                        .publish();
        //Set up observer 1
        source.subscribe(s -> System.out.println("Observer 1: " + s));
        //Set up observer 2
        source.map(String::length)
                .subscribe(i -> System.out.println("Observer 2: " + i));
        source.connect();
    }
```

3. demo  __Flowable__

```java
    public void testFlowable(){
        ConnectableFlowable<String> source =
                Flowable.just("Alpha","Beta","Gamma","Delta","Epsilon")
                        .publish();
        //Set up observer 1
        source.subscribe(s -> System.out.println("Observer 1: " + s));
        //Set up observer 2
        source.map(String::length)
                .subscribe(i -> System.out.println("Observer 2: " + i));
       source.connect();

    }
```

4. Observable  
    Observable简单来说，就是流，在RxJava中只有三个最核心的API:

    - onNext()：传递一个对象
    - onComplete()：传递完成的“信号”
    - onError()：传递一个错误

 5. RxJS 核心概念与内容概览

    - Observable (可观察对象): 表示一个概念，这个概念是一个可调用的未来值或事件的集合。

    - Observer (观察者): 一个回调函数的集合，它知道如何去监-听由 Observable 提供的值。

    - Subscription (订阅): 表示 Observable 的执行，主要用于取消 Observable 的执行。

    - Operators (操作符): 采用函数式编程风格的纯函数 (pure function)，使用像 map、filter、concat、flatMap 等这样的操作符来处理集合。

    - Subject (主体): 相当于 EventEmitter，并且是将值或事件多路推送给多个 Observer 的唯一方式。

    - Schedulers (调度器): 用来控制并发并且是中央集权的调度员，允许我们在发生计算时进行协调，例如 setTimeout 或 requestAnimationFrame 或其他。   

    [参考资料RxJava](https://zouzhberk.github.io/rxjava-study/)  
    [参考资料RxJS](https://hijiangtao.github.io/2020/01/13/RxJS-Introduction-and-Actions/) 