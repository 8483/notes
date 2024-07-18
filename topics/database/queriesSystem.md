# Database sizes

```sql
-- MySQL

SELECT table_schema AS 'Database',
       ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
GROUP BY table_schema;

-- SQL Server

SELECT
	name,
	SUM(size * 8 / 1024) AS SizeMB
FROM sys.master_files
WHERE type = 0
GROUP BY name;

-- Or this one

EXEC sp_spaceused;
```

# All tables with row counts

```sql
-- SQL Server

select *
from (
	select
		s.name schemaName,
		t.name tableName,
		sum(p.rows) rowCounts,
		max(isnull(ref.ReferencingTableCount, 0)) referencingTableCount
	from sys.tables t
		inner join sys.schemas s on s.schema_id = t.schema_id
		inner join sys.indexes i on t.object_id = i.object_id
		inner join sys.partitions p on i.object_id = p.object_id and i.index_id = p.index_id
		LEFT JOIN (
			select
				referenced_object_id,
				count(distinct parent_object_id) referencingTableCount
			 from sys.foreign_key_columns
			 group by referenced_object_id
		) ref on t.object_id = ref.referenced_object_id
	where
		t.type = 'U' -- User tables
		and i.index_id < 2 -- Filters out heaps and indexes
	group by s.name, t.name
) t1
-- where rowCounts > 0
order by
	case when referencingTableCount > 0 then 1 else 0 end desc,
    schemaName asc,
	rowCounts desc,
	referencingTableCount desc;
```

# Working set (Mimumum RAM)

Cached data pages and indexes.

```sql
-- MySQL

SELECT
	TABLE_NAME AS object_name,
    CONCAT(FLOOR(SUM(DATA_LENGTH + INDEX_LENGTH) / 1024 / 1024), ' MB') AS cached_size_MB
FROM INFORMATION_SCHEMA.TABLES
WHERE
    TABLE_SCHEMA = 'DATABASE_NAME'
    AND TABLE_TYPE = 'BASE TABLE'  -- Filter for base tables (not views)
GROUP BY TABLE_NAME
ORDER BY cached_size_MB DESC;

-- SQL Server

SELECT
    COUNT(*) * 8 / 1024 AS cached_size_MB,
    obj.name AS object_name,
    ind.name AS index_name
FROM sys.dm_os_buffer_descriptors AS bd
    INNER JOIN
    (
        SELECT object_id, index_id, allocation_unit_id
        FROM sys.allocation_units AS au
            JOIN sys.partitions AS p ON au.container_id = p.hobt_id
    ) AS pa ON bd.allocation_unit_id = pa.allocation_unit_id
    JOIN sys.objects AS obj ON pa.object_id = obj.object_id
    LEFT JOIN sys.indexes AS ind ON pa.object_id = ind.object_id
                                  AND pa.index_id = ind.index_id
WHERE bd.database_id = DB_ID('DATABASE_NAME')
GROUP BY obj.name, ind.name
ORDER BY cached_size_MB DESC;
```

# Index size (Optimal RAM)

```sql
-- MySQL

SELECT
    table_schema AS `Database`,
    table_name AS `Table`,
    ROUND(index_length / 1024 / 1024, 2) AS `Index Size (MB)`
FROM information_schema.TABLES
WHERE table_schema = 'database_name' -- <------------
ORDER BY `Index Size (MB)` DESC;

-- SQL Server

SELECT
    s.name AS [Schema],
    t.name AS [Table],
    i.name AS [Index],
    ROUND(SUM(a.total_pages) * 8 / 1024.0, 2) AS [Index Size (MB)]
FROM sys.tables AS t
    INNER JOIN sys.schemas AS s ON t.schema_id = s.schema_id
    INNER JOIN sys.indexes AS i ON t.object_id = i.object_id
    INNER JOIN sys.partitions AS p ON i.object_id = p.object_id AND i.index_id = p.index_id
    INNER JOIN sys.allocation_units AS a ON p.partition_id = a.container_id
WHERE i.type_desc <> 'HEAP'
GROUP BY s.name, t.name, i.name
ORDER BY [Index Size (MB)] DESC;
```

# Buffer Pool Hit Ratio

What % of operations are done from RAM vs disk. 100 means everything is in RAM i.e. good.

```sql
-- MySQL

Show Engine InnoDB Status -- It shows a log, look for Buffer pool hit rate

-- SQL Server

SELECT
    ROUND((CAST(a.cntr_value AS FLOAT) / b.cntr_value) * 100.0, 2) AS BufferCacheHitRatio
FROM
    sys.dm_os_performance_counters a
JOIN
    sys.dm_os_performance_counters b ON a.object_name = b.object_name
    AND a.counter_name = 'Buffer cache hit ratio'
    AND b.counter_name = 'Buffer cache hit ratio base';
```

# Find value in all tables

```sql
-- SQL Server

DECLARE @SearchValue NVARCHAR(255) = 'foo'
DECLARE @SQL NVARCHAR(MAX)

SET @SQL = ''

SELECT @SQL = @SQL +
    'SELECT ''' + TABLE_SCHEMA + '.' + TABLE_NAME + ''' AS TableName, ''' + COLUMN_NAME + ''' AS ColumnName
    FROM ' + TABLE_SCHEMA + '.' + TABLE_NAME + '
    WHERE ' + COLUMN_NAME + ' LIKE ''%' + @SearchValue + '%'' UNION ALL '
FROM INFORMATION_SCHEMA.COLUMNS
WHERE DATA_TYPE IN ('char', 'nchar', 'varchar', 'nvarchar', 'text', 'ntext')

-- Remove the last UNION ALL
IF LEN(@SQL) > 0
    SET @SQL = LEFT(@SQL, LEN(@SQL) - 10)

PRINT @SQL

-- Execute the dynamic SQL
EXEC sp_executesql @SQL
```
