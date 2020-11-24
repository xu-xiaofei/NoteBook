### 1. sql 中的空不能用 = null 来判定

__原因存在 unknown 的值__

　对 NULL 使用比较谓词后得到的结果总是 unknown 。而查询结果只会包含 WHERE 子句里的判断结果为 true 的行，不会包含判断结果为 false 和 unknown 的行。不只是等号，对 NULL 使用其他比较谓词，结果也都是一样的。所以无论 字段 是不是 NULL ，比较结果都是 unknown ，那么永远没有结果返回。以下的式子都会被判为 unknown

```sql
-- 以下的式子都会被判为 unknown
unknown = NULL  -> unknown
unknown > NULL  -> unknown
unknown < NULL  ->unknown
unknown <> NULL -> unknown
NULL = NULL  -> unknown
```

[链接](https://www.cnblogs.com/youzhibing/p/11337745.html)