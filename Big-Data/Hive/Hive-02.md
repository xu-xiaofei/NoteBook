### Hive 

### Hive是基于Hadoop 构建的一套数据仓库分析系统。
它提供了丰富的SQL查询方式来分析存储在Hadoop 分布式文件系统中的数据，可以将结构化的数据文件映射为一张数据库表，并提供完整的SQL查询功能，可以 __将SQL语句转换为MapReduce任务进行运行__ ，通过自己的SQL 去查询分析需要的内容，这套SQL 简称Hive SQL，使不熟悉mapreduce 的用户很方便的利用SQL 语言查询，汇总，分析数据。而mapreduce开发人员可以把己写的mapper 和reducer 作为插件来支持Hive 做更复杂的数据分析。
它与关系型数据库的SQL 略有不同，但支持了绝大多数的语句如DDL、DML 以及常见的聚合函数、连接查询、条件查询。HIVE不适合用于联机(online)事务处理，也不提供实时查询功能。它 __最适合应用在基于大量不可变数据的批处理作业__ 。

HIVE的特点：__可伸缩__（在Hadoop的集群上动态的添加设备），可扩展，容错，输入格式的松散耦合。

1. Hive 的数据类型
    基本类型和复杂类型  [参考](https://blog.csdn.net/bingduanlbd/article/details/52075058)  

2. Hive的分区
