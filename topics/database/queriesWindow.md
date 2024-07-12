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

# Row Number

```sql
SELECT
    ROW_NUMBER() OVER (ORDER BY profit DESC) rowNumber
FROM sales
```

# Column sum / Running total

This one is better because it can be based on all data...

```sql
select
    sum(value) over (partition by client order by date desc) runningTotal -- the `order by` turns this into a running sum.
from sales;
```

...instead of just filtered.

```sql
SELECT
    isnull(profit, 0) profit,
    SUM(profit) OVER(ORDER BY profit desc ROWS UNBOUNDED PRECEDING) AS profitRunning, -- cummulative sum of profit
    SUM(profit) OVER () AS profitTotal, -- sum of profit column
    SUM(profit) OVER(ORDER BY profit desc ROWS UNBOUNDED PRECEDING) / SUM(profit) OVER () profitShare
FROM sales
```

# Duplicates

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
