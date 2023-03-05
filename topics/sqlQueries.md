# STRING_AGG / mysql group_concat

```sql
select
	sku,
	STRING_AGG(category, ', ')
from categories
group by
    sku
```

# Only numbers

```sql
select barcode
from barcodes
where barcode not like '%[^0-9]%'
```

# Remove trailing zeros

```sql
select cast(barcode as int)
```

# Order a sub-query

Using `top 99.9999 percent` is a hack to achieve the ordering.

Note that `top 100 percent` won't work.

```sql
select
    sku,
    STRING_AGG(attribute, ', ') attributes
from (
    select top 99.9999999999999 percent
        sku,
        attribute
    from attributes
    order by
        sku asc,
        attribute asc
) t1
group by sku
```

# Declare Variables

```sql
-- regular
DECLARE @date DATETIME = '07.01.2019'

-- from select
DECLARE @count INT;
SET @count = (SELECT COUNT(sku) FROM product)
SELECT @count;
```

# UPDATE from SELECT / TABLE

SQL server

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

# Row Number

```sql
SELECT
    ROW_NUMBER() OVER (ORDER BY profit DESC) rowNumber
FROM sales
```

# Running Total

```sql
SELECT
    isnull(profit, 0) profit,
    SUM(profit) OVER(ORDER BY profit desc ROWS UNBOUNDED PRECEDING) AS profitRunning, -- cummulative sum of profit
    SUM(profit) OVER () AS profitTotal, -- sum of profit column
    SUM(profit) OVER(ORDER BY profit desc ROWS UNBOUNDED PRECEDING) / SUM(profit) OVER () profitShare
FROM sales
```

# Partition By

We use this to make a "sub-query" i.e. select n-th item from a table for further use.

### Data

```sql
SELECT
    clientID,
    invoiceDate,
    revenue
FROM sales
```

| clientID | invoiceDate | revenue  |
| -------- | ----------- | -------- |
| 1017     | 2019-01-31  | 6574.65  |
| 116      | 2018-02-05  | 5593.22  |
| 1211     | 2018-01-15  | 3637.80  |
| 116      | 2018-02-16  | 1848.00  |
| 1211     | 2018-01-09  | 15615.65 |
| 1017     | 2019-02-14  | 1386.00  |
| 1211     | 2018-02-09  | 16145.72 |
| 116      | 2018-02-13  | 2784.51  |
| 1211     | 2018-03-28  | 8844.64  |

### Data with PARTITION BY

In this case, we want to have the latest sale for each client. We can achieve this by partitioning the data i.e. ordering the sales for each client in descending order based on the date. We can then select only the desired row with `rowNumber`.

```sql
SELECT
    ROW_NUMBER() OVER (PARTITION BY clientID ORDER BY invoiceDate DESC) rowNumber,
    clientID,
    invoiceDate,
    revenue
FROM sales
```

| rowNumber | clientID | invoiceDate | revenue  |
| --------- | -------- | ----------- | -------- |
| 1         | 1017     | 2019-02-14  | 1386.00  |
| 2         | 1017     | 2019-01-31  | 6574.65  |
| 1         | 116      | 2018-02-16  | 1848.00  |
| 2         | 116      | 2018-02-13  | 2784.51  |
| 3         | 116      | 2018-02-05  | 5593.22  |
| 1         | 1211     | 2018-03-28  | 8844.64  |
| 2         | 1211     | 2018-02-09  | 16145.72 |
| 3         | 1211     | 2018-01-15  | 3637.80  |
| 4         | 1211     | 2018-01-09  | 15615.65 |

### Result

```sql
SELECT
    lastSale.clientID,
    lastSale.invoiceDate,
    lastSale.revenue
FROM (
    SELECT
        ROW_NUMBER() OVER (PARTITION BY clientID ORDER BY invoiceDate DESC) rowNumber,
        clientID,
        invoiceDate,
        revenue
    FROM sales
) lastSale
WHERE lastSale.rowNumber = 1
```

We only get the first row for each client i.e. the last sale (sorted by descending invoice date).

| acReceiver | adDate     | sales   |
| ---------- | ---------- | ------- |
| 1017       | 2019-02-14 | 1386.00 |
| 116        | 2018-02-16 | 1848.00 |
| 1211       | 2018-03-28 | 8844.64 |

### Duplicates

```sql
SELECT
    sku
FROM (
    SELECT
        ROW_NUMBER() OVER (PARTITION BY sku ORDER BY sku asc) rowNumber,
        sku
    FROM product
) duplicates
WHERE rowNumber > 1
```

# MAX, AVERAGE, SUM... Across columns

```sql
select
    *,
    (
        select max(v)
        from (
            values
                (sales2017),
                (sales2018),
                (sales2019)
        ) as value(v)
    ) as maxSales
from sales
```

# CTE - Common Table Expression

It defines a temporary result set which can be used in a SELECT statement.

```sql
WITH
    salesrep AS (
        SELECT
            employeeNumber,
            CONCAT(firstName, ' ', lastName) AS salesrepName
        FROM
            employees
        WHERE
            jobTitle = 'Sales Rep'
    ),
    customer_salesrep AS (
        SELECT
            customerName, salesrepName
        FROM
            customers
                INNER JOIN
            salesrep ON employeeNumber = salesrepEmployeeNumber
    )
SELECT
    *
FROM
    customer_salesrep
ORDER BY customerName;
```

# Rollup (Total Row)

`ROLLUP` is a subclause of the `GROUP BY` clause which provides a shorthand for defining multiple grouping sets. Unlike the `CUBE` subclause, `ROLLUP` does not create all possible grouping sets based on the dimension columns; the `CUBE` makes a subset of those.

When generating the grouping sets, `ROLLUP` assumes a hierarchy among the dimension columns and only generates grouping sets based on this hierarchy.

The `ROLLUP` is often used to generate subtotals and totals for reporting purposes.

```sql
SELECT
    brand,
    category,
    SUM(sales) sales
FROM
    sales.sales_summary
GROUP BY
    ROLLUP(brand, category)
```

This will result in

| brand | category | sales |             |
| ----- | -------- | ----- | ----------- |
| Honda | cars     | 100   |
| Honda | bikes    | 200   |
| Honda | NULL     | 300   | Subtotal    |
| BMW   | cars     | 400   |
| BMW   | bikes    | 500   |
| BMW   | NULL     | 900   | Subtotal    |
| NULL  | NULL     | 1200  | Grand Total |

# Pivot

In order for the pivoting to work, we must **only use the columns needed**, instead of all of them. This results in repeating rows.

### **Data**

Here is some example data, obtained by the given query.

| product | yearSold | revenue       |
| ------- | -------- | ------------- |
| shoes   | 2018     | 33924978.2497 |
| shirts  | 2018     | 34362105.196  |
| hats    | 2018     | 25119529.9395 |
| shoes   | 2019     | 6947797.012   |
| shirts  | 2019     | 8070039.2266  |
| hats    | 2019     | 5780623.5762  |

```sql
SELECT
    product,
    yearSold,
    revenue
FROM sales
GROUP BY
    yearSold,
    product
```

### **Result**

![TEA](../pics/sql/pivot_unpivot.png)

| product | 2019 | 2018 |
| ------- | ---- | ---- |
| shoes   | 694  | 3392 |
| shirts  | 807  | 3436 |
| hats    | 578  | 2511 |

### **Approach 1 - FROM**

```sql
SELECT
    product,  -- grouping column
    [2019],   -- spreading value 1
    [2018]    -- spreading value 2
FROM (
    SELECT
        product,      -- grouping column
        yearSold,     -- spreading column
        revenue       -- aggregation column
    FROM sales
    GROUP BY
        yearSold,
        product
) PivotData
    PIVOT (
        SUM(revenue)   -- aggregation function (aggregation column)
        FOR yearSold   -- spreading column
        IN (
            [2019],    -- spreading value 1
            [2018])    -- spreading value 2
        ) piv
```

### **Approach 2 - WITH (CTE)**

```sql
WITH PivotData AS (
    SELECT
        product,
        yearSold,
        revenue
    FROM sales
    GROUP BY
        yearSold,
        product
)

SELECT
    product,
    [2019],
    [2018]
FROM PivotData
    PIVOT (
        SUM(revenue)
        FOR [year]
        IN (
            [2019],
            [2018])
        ) piv
```

# Dynamic Pivot

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

-   Define variables
-   Define columns (**must be strings**)
-   Define SQL query
-   Execute the query

The column definition can also be done with this:

```sql
SELECT STRING_AGG(PIVOT_COLUMN, ',')
```

## **Shop rows, year columns**

```sql
-- variables
DECLARE @sql AS varchar(max)
DECLARE @pivot_list AS varchar(max) -- Leave NULL for COALESCE technique
DECLARE @select_list AS varchar(max) -- Leave NULL for COALESCE technique

-- columns
SELECT
	-- [2017], [2018], [2019], [2020]
    @pivot_list = COALESCE(@pivot_list + ', ', '') + '[' + PIVOT_COLUMN + ']',
	--ISNULL([2017], 0) as [2017], ISNULL([2018], 0) as [2018], ISNULL([2019], 0) as [2019], ISNULL([2020], 0) as [2020]
    @select_list = COALESCE(@select_list + ', ', '') + 'ISNULL([' + PIVOT_COLUMN + '], 0) as [' + PIVOT_COLUMN + ']'
FROM (
	SELECT
		cast(year(addate) as nvarchar(4)) as PIVOT_COLUMN
	FROM tHE_Move M
	where addate >= '01.01.2017'
	group by year(addate)
--	order by year(addate) asc -- doesn't work -- It works if SELECT TOP 100 or something
) as PIVOT_COLUMNS

-- query
SET @sql = '
; WITH PivotData AS (
    select
		shop,
		y AS PIVOT_COLUMN,
		sum(revenueNoVAT) revenueNoVAT
	from (
		SELECT
			SS.acName2 shop,
			year(m.addate) y,
			MI.anPVVATBase revenueNoVAT
		FROM tHE_MoveItem MI
			JOIN tHE_Move M ON MI.acKey = M.acKey
			JOIN tHE_SetSubj SS ON M.acIssuer = SS.acSubject
		WHERE m.acDocType in (
			' + '''3210''' + ',
			' + '''3220''' + ',
			' + '''3230''' + ',
			' + '''3240''' + '
		)
	) t1
	group by
		shop,
		y
)
SELECT shop, ' + @select_list + '
FROM PivotData
    PIVOT (
		sum(revenueNoVAT)
		FOR PIVOT_COLUMN
		IN (' + @pivot_list + ')
    ) piv
order by shop desc
'

-- @pivot_list
-- [2017], [2018], [2019], [2020]
-- @select_list
--ISNULL([2017], 0) as [2017], ISNULL([2018], 0) as [2018], ISNULL([2019], 0) as [2019], ISNULL([2020], 0) as [2020]

-- execution
EXEC (@sql)
```

## **Year rows, shop columns**

```sql
-- variables
DECLARE @sql AS varchar(max)
DECLARE @pivot_list AS varchar(max) -- Leave NULL for COALESCE technique
DECLARE @select_list AS varchar(max) -- Leave NULL for COALESCE technique

-- columns
SELECT
    @pivot_list = COALESCE(@pivot_list + ', ', '') + '[' + PIVOT_COLUMN + ']',
    @select_list = COALESCE(@select_list + ', ', '') + 'ISNULL([' + PIVOT_COLUMN + '], 0) AS [' + PIVOT_COLUMN + ']'
FROM (
    SELECT
		distinct SS.acName2 as PIVOT_COLUMN
	FROM tHE_Move M
		JOIN tHE_SetSubj SS ON M.acIssuer = SS.acSubject
	WHERE m.acDocType in (
		'3210',
		'3220',
		'3230',
        '3240'
	)
) AS PIVOT_COLUMNS

-- query
SET @sql = '
; WITH PivotData AS (
    select
		ym,
		shop AS PIVOT_COLUMN,
		sum(revenueNoVAT) revenueNoVAT
	from (
		SELECT
			SS.acName2 shop,
			concat(year(m.addate), ' + '''-''' + ', RIGHT(' + '''00''' + ' + CONVERT(varchar(2), DATEPART(MONTH, m.addate)), 2)) ym,
			MI.anPVVATBase revenueNoVAT
		FROM tHE_MoveItem MI
			JOIN tHE_Move M ON MI.acKey = M.acKey
            JOIN tHE_SetItem SI ON MI.acIdent = SI.acIdent
			JOIN tHE_SetSubj SS ON M.acIssuer = SS.acSubject
		WHERE m.acDocType in (
			' + '''3210''' + ',
			' + '''3220''' + ',
			' + '''3230''' + ',
			' + '''3240''' + '
		)
	) t1
	group by ym, shop
)
SELECT ym, ' + @select_list + '
FROM PivotData
    PIVOT (
        sum(revenueNoVAT)
        FOR PIVOT_COLUMN
        IN (' + @pivot_list + ')
    ) piv
order by ym desc
'

-- execution
EXEC (@sql)
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

# Multiple sums

```sql
select
    mi.acIdent,
    m.acissuer,
    datediff(day, max(m.adDate), getdate()) lastSaleDays,
    sum(CASE WHEN m.adDate >= getdate() - 30 THEN mi.anQty ELSE 0 END) soldPastDaysQty,
    sum(CASE WHEN m.adDate between getdate() - (365 * 1) and getdate() - (365 * 1 - 30) THEN mi.anQty ELSE 0 END) soldNextDaysOneYearAgoQty,
    sum(CASE WHEN m.adDate between getdate() - (365 * 2) and getdate() - (365 * 2 - 30) THEN mi.anQty ELSE 0 END) soldNextDaysTwoYearsAgoQty,
    sum(CASE WHEN m.adDate between getdate() - (365 * 3) and getdate() - (365 * 3 - 30) THEN mi.anQty ELSE 0 END) soldNextDaysThreeYearsAgoQty,
    sum(CASE WHEN m.adDate between getdate() - (365 * 4) and getdate() - (365 * 4 - 30) THEN mi.anQty ELSE 0 END) soldNextDaysFourYearsAgoQty
from the_moveitem mi
    left join the_move m on mi.acKey = m.acKey
where
    m.adDate > getdate() - 365 * 4
group by
    mi.acident,
    m.acIssuer
```
