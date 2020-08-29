GROUP BY语句通常与聚合函数（COUNT，MAX，MIN，SUM，AVG）一起使用，以将结果集按一列或多列分组。

语法：
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name

`select count(*) as tatoal from table_name group by column_name`

group by 每个分组只会展示一条数据
`select id,max(age) from student group by sex`
此时得到的结果，id 和age 并不一定是同一行的数据
也就是说聚合函数算的是每个分组内的数据，最大值也好，sum也好都是每个分组的计算结果
而不在group by 后面跟着的字段如ID，会默认取分组以后的第一条，也就是在原始表中较小的id，取完以后后面的order by 才生效，也就是说无法通过order by 来使得一整行的顺序上升或者下降
group by 后面的字段可以用在order by 中，告诉分组当中使用什么字段按什么排序
```sql
select id,max(start_date) from dict_mapping_csv where  device_model_id ='AESC_U_Process_PK1022_ACT' and identify in ('V_NUM_DATA_006','V_NUM_DATA_007','V_STR_DATA_032') group by identify order by identify DESC
```

order by 跟在group by 后面的时候必须使用group by 的字段，而不可以使用其他的未分组的字段
