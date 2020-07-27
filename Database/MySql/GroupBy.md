GROUP BY语句通常与聚合函数（COUNT，MAX，MIN，SUM，AVG）一起使用，以将结果集按一列或多列分组。

语法：
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name

`select count(*) as tatoal from table_name group by column_name`