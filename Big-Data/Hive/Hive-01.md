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

修改字段语法：  
    ```sql
    ALTER TABLE name RENAME TO new_name
    ALTER TABLE name ADD COLUMNS (col_spec[, col_spec ...])
    ALTER TABLE name DROP [COLUMN] column_name
    ALTER TABLE name CHANGE column_name new_name new_type
    ALTER TABLE name REPLACE COLUMNS (col_spec[, col_spec ...])
    ```

数据库修改列字段时候无法将较大类型的字段改为较小类型的字段，因为数据可能丢失

由此观察DDL 操作数据库表格式的语法，基本上都带table这个关键字