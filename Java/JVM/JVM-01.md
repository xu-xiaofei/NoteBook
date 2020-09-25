
# 1. 常量池相关

Java 对于基本类型和 String 采用池化的享元模式，将其放在常量池中，提高运行程序速度，更节省内存

String 有两种方式放入常量池中

1. String ab = "ab" 定义字面字符串，创建过程会先去常量池寻找此对象，若是没有则放入常量池，否则直接引用常量池的对象
2. String c = ab.intern() intern 是将ab字符串放入常量池当中并且返回地址给ｃ，如果常量池已经存在此字符串，就直接返回引用给c
此时ab 和c 的内存地址是不同的ab 在堆中，c在常量池当中

```java
    @Test
    public void testString(){

        String c = new String("TestString");
        log.info("堆里面的地址: {}", System.identityHashCode(c));
        c.intern();
        log.info("intern 共享String以后的地址: {}", System.identityHashCode(c));
        c = c.intern();
        log.info("intern 共享String以后返回的地址: {}", System.identityHashCode(c));
        String d = "TestString";
        log.info("常量池 地址: {}", System.identityHashCode(d));

    }
----------------------
2020-09-25 15:13:36.282 [main] INFO  .test.TestSort - 堆里面的地址: 306612792
2020-09-25 15:13:36.289 [main] INFO  .test.TestSort - intern 共享String以后的地址: 306612792
2020-09-25 15:13:36.289 [main] INFO  .test.TestSort - intern 共享String以后返回的地址: 1499136125
2020-09-25 15:13:36.289 [main] INFO  .test.TestSort - 常量池 地址: 1499136125
```
# 2. 不同对象是否共享常量池

 - 2.1 说明String使用“” 创建对象和非包装类型是共享常量池的
```java
    @Test
    public void testSharedPool(){

        ClassB classB = new ClassB();
        ClassA classA = new ClassA();
        ClassC classC = new ClassC();

        log.info("class A testString address {}", System.identityHashCode(classA.testString));
        log.info("class B testString address {}", System.identityHashCode(classB.testString));
        log.info("class C testString address {}", System.identityHashCode(classC.testString));

        log.info("class A integer address {}", System.identityHashCode(classA.integer));
        log.info("class B integer address {}", System.identityHashCode(classB.integer));
        log.info("class C integer address {}", System.identityHashCode(classC.integer));
    }

    @Data
    public static class ClassA {
        private String testString = new String("ABC");
        private Integer integer = new Integer(1);
    }

    @Data
    public static class ClassB {
        private String testString = "ABC";
        private int integer = 1;
    }

    @Data
    public static class ClassC {
        private String testString = "ABC";
        private int integer = 1;
    }
--------------------------------------------------------------------------
2020-09-25 15:35:05.959 [main] INFO  .TestSort - class A testString address 306612792
2020-09-25 15:35:05.964 [main] INFO  .TestSort - class B testString address 1499136125
2020-09-25 15:35:05.964 [main] INFO  .TestSort - class C testString address 1499136125
2020-09-25 15:35:05.964 [main] INFO  .TestSort - class A integer address 1926343982
2020-09-25 15:35:05.964 [main] INFO  .TestSort - class B integer address 762476028
2020-09-25 15:35:05.964 [main] INFO  .TestSort - class C integer address 762476028

```
 - 2.2 常量池是否共享数值类型的对象
    结论：赋值给对象类型的不会使用常量池，赋值给非包装类型的数值，会使用常量池；
    在拆包装类型的过程中会查询常量池是否含有此数字，如果含有则直接返回常量池的引用，若是没有则放入常量池，然后返回引用
    也就是不管怎样，常量池在拆包装以后总会有这个数值

 ```java
   @Test
    public void testSharedPool(){

        ClassB classB = new ClassB();
        ClassA classA = new ClassA();
        ClassC classC = new ClassC();

        log.info("class A integer address {}", System.identityHashCode(classA.integer));
        log.info("class B integer address {}", System.identityHashCode(classB.integer));
        log.info("class C integer address {}", System.identityHashCode(classC.integer));
    }

    @Data
    public static class ClassA {
        private Integer integer = new Integer(1);
    }

    @Data
    public static class ClassB {
        private int integer = 1;
    }
    @Data
    public static class ClassC {
        private int integer = new Integer(1);
    }
---------------------------
2020-09-25 15:40:52.420 [main] INFO TestSort - class A integer address 306612792
2020-09-25 15:40:52.424 [main] INFO TestSort - class B integer address 1499136125
2020-09-25 15:40:52.425 [main] INFO TestSort - class C integer address 1499136125
 ```
