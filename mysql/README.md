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

### Solve collation errors like 
```
sqlalchemy.exc.DatabaseError: (mysql.connector.errors.DatabaseError) 1267 (HY000): Illegal mix of collations (utf8mb4_unicode_ci,COERCIBLE) and (utf8mb4_general_ci,COERCIBLE) for operation '='
```


```mysql
# 1. find the `CHARACTER_SET_NAME` and `COLLATION_NAME` of the columns
SELECT *
FROM information_schema.`COLUMNS`
WHERE table_schema = 'SCHEMA'
  AND table_name = 'TABLE'
  AND column_name regexp 'COLUMN'
;

# 2. force using the collation
create or replace table as
select convert('string' using utf8) collate utf8_unicode_ci as column
from another_table
;
```
