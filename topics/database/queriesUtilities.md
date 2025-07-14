# Uptime

```sql
SELECT
	sqlserver_start_time,
	DATEDIFF(DAY, sqlserver_start_time, GETDATE()) days
FROM sys.dm_os_sys_info;
```

# Database sizes

```sql
-- SQL Server

SELECT
	name,
	SUM(size * 8 / 1024) AS SizeMB
FROM sys.master_files
WHERE type = 0
GROUP BY name;

-- Or this one

EXEC sp_spaceused;

-- MySQL

SELECT table_schema AS 'Database',
       ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.tables
GROUP BY table_schema;
```

# Table sizes

Check the size of all the tables in a database

```sql
SELECT
	  table_name,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) "Size (MB)"
FROM information_schema.TABLES
WHERE table_schema = "DATABASE_NAME" -- DATABASE_NAME
ORDER BY (data_length + index_length) DESC;
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
-- SQL Server

SELECT
    db_name(database_id) AS database_name,
    COUNT(*) AS cached_pages_count,
	COUNT(*) * 8 / 1024 AS cached_size_MB
FROM sys.dm_os_buffer_descriptors
WHERE database_id > 4 -- Exclude system databases
GROUP BY database_id
ORDER BY cached_pages_count DESC;

-- Per table

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
```

# List of all indexes

```sql
with
	sizes as (
		select
			object_id,
			index_id,
			used_page_count pages,
			used_page_count * 8 / 1024 index_size_mb
		from sys.dm_db_partition_stats
		where used_page_count * 8 / 1024 > 0
	)
select
	*,
	concat('DROP INDEX [', index_name, '] ON [', table_name, '];') drop_command
from (
	SELECT
		t.name table_name,
		i.type_desc index_type,
		i.name index_name,
		STRING_AGG(
			CAST(
				CASE
					WHEN ic.is_included_column = 1 THEN '[INCLUDE] ' + c.name
					ELSE c.name
				END AS VARCHAR(MAX)
			), ', '
		) WITHIN GROUP (ORDER BY ic.key_ordinal, ic.index_column_id) AS columns_in_index,
		i.is_primary_key,
		i.is_unique_constraint,
		max(pages) pages,
		max(index_size_mb) index_size_mb,
		max(o.create_date) created,
		max(o.modify_date) modified
	FROM sys.indexes i
		JOIN sys.objects o on o.object_id = i.object_id
		JOIN sys.tables t ON i.object_id = t.object_id
		JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id
		JOIN sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
		LEFT JOIN sizes s on s.object_id = i.object_id AND s.index_id = i.index_id
	WHERE i.name IS NOT NULL
	GROUP BY
		t.name,
		i.name,
		i.type_desc,
		i.is_primary_key,
		is_unique_constraint
) t1
ORDER BY
    table_name ASC,
    index_type ASC;
```

# Index size (Optimal RAM)

```sql
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

-- MySQL

SELECT
    table_schema AS `Database`,
    table_name AS `Table`,
    ROUND(index_length / 1024 / 1024, 2) AS `Index Size (MB)`
FROM information_schema.TABLES
WHERE table_schema = 'database_name' -- <------------
ORDER BY `Index Size (MB)` DESC;
```

# Frequently Accessed Indexes

Most accessed indexes. High `USER_SEEKS` or `USER_SCANS` indicates frequent reads.

```sql
-- SQL Server
SELECT
    OBJECT_NAME(S.[OBJECT_ID]) AS TableName,
    I.[NAME] AS IndexName,
    USER_SEEKS, USER_SCANS, USER_LOOKUPS, USER_UPDATES
FROM SYS.DM_DB_INDEX_USAGE_STATS AS S
INNER JOIN SYS.INDEXES AS I
    ON I.[OBJECT_ID] = S.[OBJECT_ID] AND I.INDEX_ID = S.INDEX_ID
WHERE OBJECTPROPERTY(S.[OBJECT_ID], 'IsUserTable') = 1
ORDER BY USER_SEEKS DESC;
```

# Frequently Accessed Tables

Most accessed tables. Table access frequency in the plan cache.

```sql
SELECT
    qs.execution_count,
    qs.total_logical_reads AS Reads,
    qs.total_logical_writes AS Writes,
    SUBSTRING(st.text, (qs.statement_start_offset/2)+1,
              ((CASE qs.statement_end_offset
                  WHEN -1 THEN DATALENGTH(st.text)
                  ELSE qs.statement_end_offset
                END - qs.statement_start_offset)/2)+1) AS QueryText
FROM sys.dm_exec_query_stats qs
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) st
ORDER BY qs.total_logical_reads DESC;
```

# Buffer Pool Hit Ratio

What % of operations are done from RAM vs disk. 100 means everything is in RAM i.e. good.

```sql
-- SQL Server

SELECT
    ROUND((CAST(a.cntr_value AS FLOAT) / b.cntr_value) * 100.0, 2) AS BufferCacheHitRatio
FROM
    sys.dm_os_performance_counters a
JOIN
    sys.dm_os_performance_counters b ON a.object_name = b.object_name
    AND a.counter_name = 'Buffer cache hit ratio'
    AND b.counter_name = 'Buffer cache hit ratio base';

-- MySQL

Show Engine InnoDB Status -- It shows a log, look for Buffer pool hit rate
```

# Expensive queries

```sql
-- SQL Server

select *
from (
	SELECT TOP 100
		last_execution_time,
		dest.text,
		round((deqs.total_elapsed_time / deqs.execution_count) / 1000000, 0) AS avg_elapsed_time_seconds,
		round((deqs.total_worker_time / deqs.execution_count) / 1000000, 0) AS avg_worker_time_seconds,
		deqs.execution_count,
		round(deqs.total_elapsed_time / 1000000, 0) AS total_elapsed_time_seconds,
		round(deqs.total_worker_time / 1000000, 0) AS total_worker_time_seconds
	FROM sys.dm_exec_query_stats AS deqs
		CROSS APPLY sys.dm_exec_sql_text(deqs.sql_handle) AS dest
	WHERE deqs.last_execution_time >= DATEADD(hour, -8, GETDATE())
	ORDER BY
		avg_elapsed_time_seconds DESC
) t1
order by avg_elapsed_time_seconds desc
;
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
