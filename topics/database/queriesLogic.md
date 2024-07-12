# Coalesce

The SQL Server `COALESCE` function handles `NULL` values.

The expression accepts a number of arguments, evaluates them in sequence, and returns the first non-null argument.

```sql
SELECT COALESCE (NULL,'A','B')                     -- A
SELECT COALESCE (NULL,100,20,30,40)                -- 100
SELECT COALESCE (NULL,NULL,20,NULL,NULL)           -- 20
SELECT COALESCE (NULL,NULL,NULL,NULL,NULL,'foo')   -- foo
SELECT COALESCE (NULL,NULL,NULL,NULL,1,'foo')      -- 1
SELECT COALESCE (NULL,NULL,NULL,NULL,NULL,'foo',1) -- coversion failed when converting the varchar value 'foo' to data type int
```

# Recursion

Dates generator from date to date (now).

```sql
with recursive dates as (
	select '2023-01-01' date -- start date
	union all
	select date_add(date, interval 1 day)
	from dates
	where date < curdate() -- end date
)
select * from dates

/*
2023-01-01
2023-01-02
2023-01-03
...
*/
```
