### Spring boot 中的事务开启

1. 
@Transactional 注解的属性信息
属性名	说明
name	当在配置文件中有多个 TransactionManager , 可以用该属性指定选择哪个事务管理器。
propagation	事务的传播行为，默认值为 REQUIRED。
isolation	事务的隔离度，默认值采用 DEFAULT。
timeout	事务的超时时间，默认值为-1。如果超过该时间限制但事务还没有完成，则自动回滚事务。
read-only	指定事务是否为只读事务，默认值为 false；为了忽略那些不需要事务的方法，比如读取数据，可以设置 read-only 为 true。
rollback-for	用于指定能够触发事务回滚的异常类型，如果有多个异常类型需要指定，各类型之间可以通过逗号分隔。
no-rollback- for	抛出 no-rollback-for 


2. 七种事务传播行为

事务传播行为用来描述由某一个事务传播行为修饰的方法被嵌套进另一个方法的时事务如何传播。

2. Spring中七种事务传播行为
事务传播行为类型	说明
PROPAGATION_REQUIRED	如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。这是最常见的选择。
PROPAGATION_SUPPORTS	支持当前事务，如果当前没有事务，就以非事务方式执行。
PROPAGATION_MANDATORY	使用当前的事务，如果当前没有事务，就抛出异常。
PROPAGATION_REQUIRES_NEW	新建事务，如果当前存在事务，把当前事务挂起。
PROPAGATION_NOT_SUPPORTED	以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
PROPAGATION_NEVER	以非事务方式执行，如果当前存在事务，则抛出异常。
PROPAGATION_NESTED	如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。

[参考](https://juejin.im/entry/5a8fe57e5188255de201062b)

3. 事务的隔离度

---以上手册中的理论知识;
===========================================================================================
       隔离级别               脏读（Dirty Read）          不可重复读（NonRepeatable Read）     幻读（Phantom Read）
===========================================================================================

未提交读（Read uncommitted）        可能                            可能                       可能

已提交读（Read committed）          不可能                          可能                        可能

可重复读（Repeatable read）          不可能                          不可能                     可能

可串行化（Serializable ）                不可能                          不可能                     不可能

===========================================================================================

·未提交读(Read Uncommitted)：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

·提交读(Read Committed)：只能读取到已经提交的数据。Oracle等多数数据库默认都是该级别 (不重复读)

·可重复读(Repeated Read)：可重复读。在同一个事务内的查询都是事务开始时刻一致的，InnoDB默认级别。在SQL标准中，该隔离级别消除了不可重复读，但是还存在幻象读

·串行读(Serializable)：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞

[参考](https://www.cnblogs.com/zhoujinyi/p/3437475.HTML)
