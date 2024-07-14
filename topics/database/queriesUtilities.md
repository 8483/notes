# Declare Variables

```sql
-- regular
DECLARE @date DATETIME = '07.01.2019'

-- from select
DECLARE @count INT;
SET @count = (SELECT COUNT(sku) FROM product)
SELECT @count;
```

# YYYY-MM date format

```sql
-- 2.5 million rows

-- 12 sec
select date
from sales

-- 13 sec
select concat(year(date), '-', RIGHT('00' + CONVERT(varchar(2), DATEPART(MONTH, date)), 2)) ym
from sales

-- 15 sec
select CAST(YEAR(date) AS VARCHAR(4)) + '-' + RIGHT('00' + CAST(MONTH(date) AS VARCHAR(2)), 2) as ym
from sales

-- 29 sec
select FORMAT(date, 'yyyy-MM') as ym
from sales
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

# Previous value

```sql
select
    lag(value) over (order by created) -- returns the previous row's value
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
