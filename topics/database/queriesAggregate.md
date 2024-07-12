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

# STRING_AGG / mysql group_concat

```sql
select
	sku,
	STRING_AGG(category, ', ')
from categories
group by
    sku
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
