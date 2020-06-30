# MySQL/MariaDB Tricks

select last 5 records of each id
```mysql
WITH ranked_table AS (
  SELECT t.*, ROW_NUMBER() OVER (PARTITION BY id ORDER BY datetime_col DESC) AS rn
  FROM table AS t
)
SELECT * 
FROM ranked_table
WHERE rn <= 5
;
```
