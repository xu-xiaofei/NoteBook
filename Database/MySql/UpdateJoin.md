UPDATE T1, T2,
[INNER JOIN | LEFT JOIN] T1 ON T1.C1 = T2. C1
SET T1.C2 = T2.C2, 
    T2.C3 = expr

 注意：在update 和 se 之间的 表明决定了更新的范围是多大，但不全是，因为存在外键级联更新的情况   

示例 :
```sql
UPDATE
	ledger_device_auxiliary_facilities t5
inner join (
	SELECT
		t2.id,
		CASE
			t2.create_time when '21-53' THEN 1
			when '22-12' THEN 2
			when '22-13' THEN 3
			when '22-14' THEN 4
		end as instrument_type
	from
		(
		SELECT
			t1.id, DATE_FORMAT(t1.create_time, '%H-%i') as create_time
		from
			ledger_device_auxiliary_facilities t1 ) t2 ) t3 on
	t3.id = t5.id set
	t5.instrument_type = t3.instrument_type

```