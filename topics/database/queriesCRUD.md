# UPDATE from SELECT / TABLE

SQL Server

```sql
-- select
UPDATE product
SET product.price = t2.newPrice
FROM (
    SELECT sku, newPrice
    FROM pricelist
) t2
WHERE product.sku = t2.sku

-- table
UPDATE product
SET product.price = t2.newPrice
FROM t2
WHERE product.sku = t2.sku

-- where
update product set isActive = 0
where sku in (
    select sku
    from stock
    group by sku
    having sum(quantity) = 0
)
```

MySQL

```sql
update
    product p,
    (
        select sku, newPrice
        from pricelist
    ) t2
set product.price = t2.newPrice
where product.sku = t2.sku
```

# INSERT from SELECT

```sql
-- Whole table
INSERT INTO table2
SELECT * FROM table1
WHERE condition;

-- Specific columns
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;

-- Select
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1 t1
    LEFT JOIN table2 t2 ON t2.foo = t1.foo
    LEFT JOIN table3 t3 ON t3.bar = t1.bar
WHERE
    baz = `qux`
```

# UPSERT / ON DUPLICATE KEY UPDATE

SQL Server

```sql
IF EXISTS (
    select *
    from tableName
    where
        sku = @sku
        and warehouse = '00051'
)
BEGIN
    UPDATE tableName
    SET location = @location
    WHERE
        sku = @sku
        and warehouse = '00051';
END
ELSE
BEGIN
    INSERT INTO tableName (code, location, warehouse)
    VALUES (@sku, @location, '00051');
END;
```

MySQL

```sql
-- values
insert into inventory_stock
    (
        tenantId,
        objectId,
        itemId,
        stock
    )
values
    (
        @tenantId,
        @objectId,
        @itemId,
        @updatedStock
    ) as stock
on duplicate key update
    stock = stock.stock

-- select
insert into inventory_stock
    (
        tenantId,
        objectId,
        itemId,
        stock
    )
select *
from stock
on duplicate key update
    stock = stock.stock
```

# DELETE from SELECT

```sql
delete from product
where id in (
    select id
    from t2
    where condition = 'foo'
)
```

# DELETE from LEFT JOIN

```sql
DELETE t1 -- Just from table1. DELETE t1, t2 for both tables
FROM table1 t1
	LEFT JOIN table2 t2 ON t1.id = t2.id
WHERE
	t1.criteria = 'foo' AND t2.criteria = 'bar'
```

# Bulk insert

Fastest, but has a limit of 1,000 inserts.

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz'),
        ('foo', 'bar', 'baz'),
        ('foo', 'bar', 'baz')
    ;
`);
```

Best

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');

    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');

    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);
```

Slowest

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);

await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);

await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);
```
