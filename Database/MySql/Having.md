HAVING子句已添加到SQL，因为WHERE关键字不能与聚合函数一起使用。

具体语法
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value

```sql
select * from sys_batch_log having count(*) >1
// 这最多只会得到一条结果，由此可以看出having 和where 本质上有相似之处，只是having支持聚合函数
```

简单点说，逻辑值有三种，只有为真才会继续走下去，而unknown 和false 就不往下走了