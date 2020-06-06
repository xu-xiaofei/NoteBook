### 并发工具类

1. 闭锁CountDownLatch
CountDownLatch 允许一个或多个线程等待一些特定的操作完成，而这些操作是在其它的线程中进行的，也就是说有明确的 等待的线程 和 被等的线程 ；
CountDownLatch 构造函数中有一个 count 参数，表示有多少个线程需要被等待，对这个变量的修改是在其它线程中调用 countDown 方法，每一个不同的线程调用一次 countDown 方法就表示有一个被等待的线程到达，count 变为 0 时，latch（门闩）就会被打开，处于等待状态的那些线程接着可以执行；
CountDownLatch 是一次性使用的，也就是说latch门闩只能只用一次，一旦latch门闩被打开就不能再次关闭，将会一直保持打开状态，因此 CountDownLatch 类也没有为 count 变量提供 set 的方法；

2. 栅栏CyclicBarrier  
一组线程做一个组合的事情，循环如此，step by step，可以使用多次。
是条件满足以后，各个线程一起开始做各自的事情，难以将各个子线程进行工作排序。

3. 信号量Semaphore

For semZero all acquire() calls will block and tryAcquire() calls will return false, until you do a release()
For semOne the first acquire() calls will succeed and the rest will block until the first one releases.