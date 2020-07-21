

2. Hive的分区
   ```
      partitioned by(分区名 数据类型)
   ```
   - 2.1 静态分区(插入数据的时候指定分区名，没有会创建)
      可以建立多级分区
      ```sql

      ```
   - 2.2 动态分区

      ```sql
         //预先准备，因为分区以后文件可能很多，hive默认分区最大值是100
         SET hive.exec.dynamic.partition=true;
         // 如果不选择non-strict 模式，那么必须先有静态分区，才能动态分区
         SET hive.exec.dynamic.partition.mode=non-strict;
         SET hive.exec.max.dynamic.partitions=100000;
         SET hive.exec.max.dynamic.partitions.pernode=20000; 

         //创建一个临时表存储数据
         create table if not exists part_tmp
         (
            uid int,
            uname string,
            age int,
            country string
         )
         row format delimited fields terminated by '\t';
         ​
         //向临时表中存数据 
         insert into part_tmp select * from part1;
         ​
         //创建动态分区表
         create table if not exists dy_part
         (
            uid int,
            uname string,
            age int
         )
         partitioned by(country string)
         row format delimited fields terminated by '\t';
         ​
         //在给表进行数加载的时候,不会使用load命令，直接指定country的实际值
         //通过另外一张表中的列来对当前的分区进行赋值
         insert into dy_part partition(country) select * from part_tmp;
      ```
   - 2.3 混合分区

   [参考](https://zhuanlan.zhihu.com/p/80166949)

