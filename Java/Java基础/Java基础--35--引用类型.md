### Java 具有四种引用类型

1. 强引用 StrongReference
    即使报OOM也不会回收， StrongReference是Java的默认引用形式，使用时不需要显示定义。任何通过强引用所使用的对象不管系统资源有多紧张，Java GC都不会主动回收具有强引用的对象。

2. 软引用 SoftReference
    如果一个对象只具有软引用，Java GC在内存充足的时候不会回收它，内存不足时才会被回收。
    
    用途：___缓存___

3. 弱引用 WeakReference
     如果一个对象只具有弱引用，无论内存充足与否，Java GC后对象如果只有弱引用将会被自动回收。


4. 虚引用
    从PhantomReference类的源代码可以知道，它的get()方法无论何时返回的都只会是null。所以单独使用虚引用时，没有什么意义，需要和引用队列ReferenceQueue类联合使用。当执行Java GC时如果一个对象只有虚引用，就会把这个对象加入到与之关联的ReferenceQueue中。

    用途： 可用来在对象被回收时做额外的一些资源清理或事物回滚等处理。


[参考](https://juejin.im/post/5a5129f5f265da3e317dfc08)