# Collections of SQL Tricks


[mysql] select last 5 records of each id

```mysql
WITH ranked_messages AS (
  SELECT m.*, ROW_NUMBER() OVER (PARTITION BY id ORDER BY datetime_col DESC) AS rn
  FROM messages AS m
)
SELECT * 
FROM ranked_messages 
WHERE rn <= 5
;
```