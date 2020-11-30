# __Delete  with join 是极具危险性的操作！__

DELETE T1, T2
FROM T1
INNER JOIN T2 ON T1.key = T2.key
WHERE condition;


注意：在Delete 和 from 之间的 决定了删除的范围是多大，但不全是，因为存在外键级联删除的情况   
