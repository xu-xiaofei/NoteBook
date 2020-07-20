1. Hive 行列转换
    hive中的concat，concat_ws，collect_set 用法：
    collect_set的作用：collect_set(col)函数只接受基本数据类型，它的主要作用是将某字段的值进行去重汇总，产生array类型字段。

    concat_ws的作用：表示concat with separator,即有分隔符的字符串连接，concat_ws(”,collect_set(home_location))表示用空的字符”来连接collect_set返回的array中的每个元素。

    concat：可以连接一个或者多个字符串，select concat(‘11’,’22’,’33’);//112233

2. 行转列
```sql
select max(sno), name
   , concat_ws(',', collect_set(DEPART)) as DEPART 
from students_info
group by name
```

3. 列转行
```sql
select SNO, name, add_DEPART
from students_info si 
lateral  view explode(split(si.DEPART,','))  b AS add_DEPART

```
