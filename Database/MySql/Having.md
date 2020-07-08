HAVING子句已添加到SQL，因为WHERE关键字不能与聚合函数一起使用。

具体语法
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);