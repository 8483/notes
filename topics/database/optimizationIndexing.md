[source](https://databasedive.com/mysql-indexing-best-practices-779282b0995b)

# Terminology

- **heap** - Table without a clustered index. Data is unordered. Row lookups use RID (Row Identifier). Slower for large reads or key lookups.
- **clustered index** - The actual table itself is the "index" (one per table). Defines physical row order. Faster for range queries and order-based filters.
- **scan** - Full traversal of table or index. Slow. Used when predicate is broad or no usable index exists.
- **lookup** - Extra read to fetch non-indexed columns. Occurs when index lacks needed data, forcing access to the base table or clustered index.

---

- **nonclustered index** - Separate structure with pointers to data (heap or clustered). Can have many per table. Faster for specific column searches.
- **index include** - Extra non-key columns stored in the index leaf level. Eliminates lookups by covering more queries. Improves read performance.
- **seek** - Targeted access using index keys. Fast. Narrow predicate matches specific index values.

# What is an index?

> A copy of table that is pre-sorted by commonly used columns. Ex. First name, last name, sku...

It's like the index of a book. It has a list of words or phrases in alphabetical order, which makes searching easier and faster. Then it has pages number pointing you to where the words appear in the actual content of the book.

A database index is an extra bit added to the database that has the specified fields sorted in some way for quick and easy searching, and it points back to the actual record in the database.

To give a more concrete example, imagine you have a book (like the telephone directory) which is laid out with all the entries sorted by **last name** and then **first name**. If you're trying to find an entry for someone whose **last name** starts with a `K`, that's nice and simple. However, what happens if you want to find all the people with the **first name** Joshua? You'd have to literally read through every entry on every page and mark the ones you're interested in.

If the telephone book also had an index of **first name**s - so it tells you the first Joshua is the 22nd entry on page 2, and so on - it would make this task _much_ faster and more efficient. This is exactly what an index in a database does. It obviously only works for particular queries, because this hypothetical first name index isn't going to help you find the person who lives at the **address** 21 Jump Street, so you have to set up your indexes according to what queries you want to speed up.

It is an extra table telling the db engine where to find a primary key. It reduces search times by being sorted in a specific way, and often has some extra, useful information.

If you have a table with products, and you have maybe 5-8 columns, and products are not sorted in any specific way, just added one after another.

And you realize that users often give a product number and want to get the name. So instead of having to load the large table, and traverse it looking for this id, you design it so.

That such search goes to the index, which only has PK, prod Nr and name, and is sorted ASC.

The search is much quicker.

But indexes can also mess a db up, if they are too old, long, complex etc.

# How indexing works

> A MySQL select query consists of two phases. A query phase and a fetch phase. In the query phase, MySQL tries to find primary key values of all matching rows using the index. In the fetch phase, MySQL uses these retrieved primary keys to fetch the actual row values.

If you have a MySQL with InnoDB engine, you will commonly use indexes such as **Clustered index** and **secondary (non-clustered) index**.

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

# When to index

- Do not create indexes unless you know you'll need them.
- Don't index each column in the table separately.

> MySQL can only use one index per table in a query, for all WHERE, JOIN, GROUP BY and ORDER BY. It's a common mistake to think that you need an index on each of the columns used by them.

Ideally, query performance tuning should happen regularly.
Indexing is not a one-time process. It is advised to conduct weekly or monthly checks of database performance to prevent issues adversely affecting your applications.

What to index?

- Columns you're going to query.
- Columns you'll JOINs on.
- Columns you ORDER/GROUP BYs.

# Check if there is an index

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

**Column ordering matters!** The column which has the smallest cardinality, i.e. The least distinctive values, should always be positioned as the left-most side in a composite index. Applies for the rest of the columns.

To find the cardinality, use `DISTINCT`.

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

# Notes

make sure that the first column is something that you are searching for

it doesnt matter if the values are index keys or includes, the size difference is negligeble.

WHERE without a supporting index (with first key as the search value) = table scan

ORDER BY without a supporting index = CPU work to rebuild pages

SQL Server caches pages, not query results

Singleton lookups are key lookups that return exactly one row.

Occur when a nonclustered index partially satisfies a query.

SQL Server uses the index to locate the row in the base table or clustered index to fetch additional columns.

Returning a single row is more efficient than scanning, but still adds overhead compared to a fully covering index.
