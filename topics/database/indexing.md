[source](https://databasedive.com/mysql-indexing-best-practices-779282b0995b)

# General

> MySQL can only use one index per table in a query, for all WHERE, JOIN, GROUP BY and ORDER BY. It's a common mistake to think that you need an index on each of the columns used by them.

Ideally, query performance tuning should happen regularly.
Indexing is not a one-time process. It is advised to conduct weekly or monthly checks of database performance to prevent issues adversely affecting your applications.

# How indexing it works

> A MySQL select query consists of two phases. A query phase and a fetch phase. In the query phase, MySQL tries to find primary key values of all matching rows using the index. In the fetch phase, MySQL uses these retrieved primary keys to fetch the actual row values.

If you have a MySQL with InnoDB engine, you will commonly use indexes such as **Clustered index** and **secondary index**.

> When you define a PRIMARY KEY on your table, InnoDB uses it as the clustered index. InnoDB table storage is organized based on the values of the primary key columns, to speed up queries and sorts involving the primary key columns. All indexes other than the clustered index are known as secondary indexes.

In InnoDB, each record in a secondary index contains the primary key columns for the row, as well as the columns specified for the secondary index. InnoDB uses this primary key value to search for the row in the clustered index.

So, MySQL has to depend on the Primary Key index to perform the fetch phase even if all the matching rows are identified using an index (Secondary index).

```sql
select mobile from users where mode = 2
```

Consider the above query and assume you have created an index on 'mode'.
MySQL can easily retrieve the Primary Key values for all the records with mode value as 2, however, to fetch the mobile column, MySQL would still need to use the primary key values to fetch row data from the Primary Key index.

Now, what if we add an index like this...

```sql
ALTER TABLE users ADD INDEX index_covering(mode, mobile)
```

Using above index, MySQL can easily retrieve the Primary Key values for all the records with mode value as 2, and to fetch the mobile column, MySQL no need to depend on the Primary Key index to fetch row data. Above query is completely covered by the index and hence known as a covering index.

> The ideal database design uses a covering index where practical; the query results are computed entirely from the index, without reading the actual table data.

# Rules

So what columns should I Index? It depends…

-   What columns you're going to query
-   What JOINs you'll perform
-   What ORDER/GROUP BYs etc.

Do's and don'ts

-   Do not create indexes unless you know you'll need them.
-   Don't index each column in the table separately.
-   Avoid using functions on the Left Hand-Side of the Operator. It's ok to have a function in the right hand.

```sql
select * from users where upper(email) = 'foo@bar.com';
-- vs
select * from users where created < now();
```

-   Avoid Wildcard Characters at the Beginning of a LIKE Pattern. MySQL won't use an index for wildcard searches.

```sql
select * from users where email LIKE "%foo%";
-- vs
select * from users where email LIKE "foo%";
```

-Avoid OR conditions and use UNION as an alternative.
-Avoid sorting with a mixed order.
-In where clause, compare a column to a value which matches the column’s type.

```sql
select * from users where email = 101;
-- vs
select * from users where email = "101";
```

-Columns on each side of ON clause of a join must be of the same type.

-   If columns on each side of ON clause of a join is VARCHAR, make sure the collations of each of the columns are identical.
-   You do not want to place too many indexes on tables that are frequently updated
-   Keep your table statistics up to date. Done with `ANALYZE TABLE table_name`

# Identifying slow queries

1. MySQL Slow query log. Enable it.
2. processlist. Use it.

```sql
show processlist;
show full processlist;
```

# Is there an index?

You can see if queries are slow **only** due to the lack of an index with `EXPLAIN`, which shows the query execution plan i.e. which index to pick, how to join tables and in which order for optimal performance.

```sql
EXPLAIN select * from users where email='foo@bar.com';
```

`rows` shows that 152,685 rows were traversed. `possible_keys` tells us what are the available indexes in a table or the indexes which may be used for our queries. `key` tells us what are the indexes that MySQL used for the analyzed query. As of now, both are empty. An empty `key` column provides you with the information that the query lacks an index.

```
+----+-------------+--------+------------+-------+---------------+------+---------+-------+--------+----------+--------------+
| id | select_type | table  | partitions | type  | possible_keys | key  | key_len | ref   | rows   | filtered | Extra        |
+----+-------------+--------+------------+-------+---------------+------+---------+-------+--------+----------+--------------+
|  1 | SIMPLE      | users  | NULL       | all   | NULL          | NULL | NULL    | NULL  | 152865 |    10.00 | using where  |
+----+-------------+--------+------------+-------+---------------+------+---------+-------+--------+----------+--------------+
```

# Adding an index

```sql
ALTER TABLE users ADD INDEX index_email(email)
```

The query is now optimized and MySQL was able to find a matching result without traversing many rows. If you notice, we have our index name present in both of the columns `possible_keys` and `key`.

```
+----+-------------+--------+------------+-------+---------------+----------------+---------+--------+------+----------+---------+
| id | select_type | table  | partitions | type  | possible_keys | key            | key_len | ref    | rows | filtered | Extra   |
+----+-------------+--------+------------+-------+---------------+----------------+---------+--------+--------+----------+-------+
|  1 | SIMPLE      | users  | NULL       | ref   | index_email   | index_email    | 387     | const  | 1    |   100.00 | NULL    |
+----+-------------+--------+------------+-------+---------------+----------------+---------+--------+------+----------+---------+
```

# Composite index (Multiple columns index)

> Keeping the rule of thumb in mind, you know that creating two indexes on email and category will not help.

```sql
select * from users where email='foo@bar.com' and category_id = 3;
```

> If separate single-column indexes exist on col1 and col2, MySQL optimizer attempts to use the Index Merge optimization or attempts to find the most restrictive index by deciding which index excludes more rows and then uses that index to fetch the rows.

By adding several columns into an index you can narrow down the number of rows MySQL has to traverse to find matching rows. An index may consist of up to 16 columns.

```sql
ALTER TABLE users ADD INDEX index_co1_col2(column1, column2, column3,...)
```

**Column ordering matters!**. The column which has the smallest cardinality, i.e. The least distinctive values, should always be positioned as the left-most side in a composite index. Applies for the rest of the columns.

To find the cardinality, use `DISTINCT`

```sql
select count(distinct(email)) from users;         -- 138,279
select count(distinct(category_id)) from users;   -- 16

-- This index is useful only for this query.
ALTER TABLE users ADD INDEX index_category_email(category_id, email)
```

If the table has a multiple-column index, any leftmost prefix of the index can be used by the optimizer to look up rows. For example, if you have a three-column index on (col1, col2, col3), you have indexed search capabilities on (col1), (col1, col2), and (col1, col2, col3).

> This means that MySQL optimizer can't use an index to perform lookups if the query doesn't contain the leftmost prefix column of the index. So the left-most column in a composite index is known as the Lookup column.

In other words, MySQL can use multiple-column indexes for queries that test all the columns in the index, or queries that test just the first column, the first two columns, the first three columns, and so on. **If you specify the columns in the right order in the index definition**, a single composite index can speed up several kinds of queries on the same table.

> if you have a three-column index on (col1, col2, col3), you have indexed search capabilities on (col1), (col1, col2), and (col1, col2, col3)

```sql
ALTER TABLE users ADD INDEX index_status_category_email(status, category_id, email)

-- useful for
select * from users where status = 1;
select * from users where status = 1 and category_id = 3;
select * from users where status = 1 and category_id = 3 and email = "foo@bar.com";

-- NOT useful
select * from users where category_id = 3;
select * from users where category_id = 3 and email = "foo@bar.com";
```

# What if the query contains OR instead of AND?

> In this case, MySQL won't be able to use the index on queries having an OR condition, even if the query contains lookup column and maintains the order of WHERE clause same as the index.

Therefore, it's recommended to avoid such OR conditions and consider splitting the query to two parts, combined with a UNION DISTINCT (or even better, UNION ALL, in case you know there won't be any duplicate results)

```sql
select status, category_id from users where status = 1
UNION ALL
select status, category_id from users where category_id = 3
```

# What if the query contains GROUP BY and ORDER BY?

```sql
-- index(status, category_id, email)
select * from users where status = 1 ORDER BY category_id
```

This will use the composite index for the WHERE clause.

If you think this will narrow down the records with the status having value as 1, sadly you now need to perform a sort on these resulting records to get them sorted by category_id.

This is because the index didn't sort the results by category_id in any meaningful way and ORDER BY clause lacks the lookup column.

This is known as a File Sort (some of you might have noticed this in an explain result). This happens because the index that we created failed to satisfy for the ORDER BY clause.

> File Sort: A sort that occurs after the query; it requires fetching the data into a temporary buffer and sorting it before finally returning. This wouldn't have been needed if the data was already sorted by the index in the way you wanted.

```sql
select * from users where status = 1 ORDER BY status, category_id
```

This query can leverage the usage of the mentioned index because it qualifies for both the WHERE clause and ORDER BY clause.

The same is also applicable to GROUP BY statements. if you run the following query with the composite index on (status, category_id, email):

```sql
select * from users where status = 1 GROUP BY category_id
```

The records are already sorted by status, category_id and email. This allows you to quickly filter down all the records with status = 1. After these results are returned they are also then sorted based on category_id since the index orders the rows differently than required in the query.

# What if the query contains JOINS?

> You should have indexes on all the columns used in the JOIN clauses. i.e, columns on each side of ON clause of a join must be indexed.
