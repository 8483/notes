# Best practices

-   Always foreign key to IDs, rather than column values.
-   Try to stick to `where` clauses on indexed columns, instead of `like`.
-   Don't go crazy with `joins`.
-   Don't use varchar(255). Try to use the lowest number possible.
-   Avoid using functions on the Left Hand-Side of the Operator. It's ok to have a function in the right hand.

```sql
select * from users where upper(email) = 'foo@bar.com';
-- vs
select * from users where created < now();
```

-   Avoid Wildcard Characters at the Beginning of a LIKE Pattern. MySQL won't use an index for wildcard searches.

```sql
select * from users where email LIKE '%foo%';
-- vs
select * from users where email LIKE 'foo%';
```

-   Avoid OR conditions and use UNION as an alternative.
-   Avoid sorting with a mixed order.
-   In where clause, compare a column to a value which matches the column’s type.

```sql
select * from users where email = 101;
-- vs
select * from users where email = '101';
```

-   Columns on each side of ON clause of a join must be of the same type.
-   If columns on each side of ON clause of a join is VARCHAR, make sure the collations of each of the columns are identical.
-   You do not want to place too many indexes on tables that are frequently updated
-   Keep your table statistics up to date. Done with `ANALYZE TABLE table_name`

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
