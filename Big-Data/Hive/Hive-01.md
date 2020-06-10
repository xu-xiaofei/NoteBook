建表指定分隔符  
`create table test(uid string,name string)row format delimited fields terminated by ‘/t’；`

修改分割符  
`alter table store set SERDEPROPERTIES('field.delim'='\t');`


hive删除表：  
`drop table table_name;`  
hive删除表中数据：  
`truncate table table_name;`  
hive按分区删除数据：  
`alter table table_name drop partition (partition_name='分区名')`  

Hive 不支持delete 和update的操作在{}版本之前