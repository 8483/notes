# List of all indexes with various stats

```sql
select
	*,
	concat('ALTER INDEX ', index_name, ' ON ', table_name, ' REBUILD WITH (FILLFACTOR = 80);') rebuild_command,
	concat('DROP INDEX [', index_name, '] ON [', table_name, '];') drop_command
from (
	SELECT
		t.name table_name,
		i.type_desc index_type,
		i.name index_name,

		STRING_AGG(
			CAST(
				CASE
					WHEN ic.is_included_column = 0 THEN c.name
					ELSE '[INCLUDE] ' + c.name
				END AS VARCHAR(MAX)
			), ', '
		) WITHIN GROUP (ORDER BY ic.is_included_column, ic.key_ordinal, ic.index_column_id) columns_in_index,

        count(c.name) columns_count,

		isnull(max(used_page_count * 8 / 1024), 0) index_size_mb,

		isnull(max(used_page_count), 0) pages,
		round(max(frag.avg_fragmentation_in_percent), 0) frag_percent,
		round(max((frag.avg_fragmentation_in_percent / 100) * used_page_count), 0) fragged_pages,
		max(case when frag.avg_fragmentation_in_percent > 30 then 'yes' else 'no' end) should_rebuild,

		max(o.create_date) created,
		max(o.modify_date) modified,

		i.is_primary_key,
		i.is_unique_constraint

	FROM sys.indexes i
		JOIN sys.objects o on o.object_id = i.object_id
		JOIN sys.tables t ON i.object_id = t.object_id
		JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id
		JOIN sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
		JOIN sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') frag on frag.object_id = i.object_id and frag.index_id = i.index_id
		JOIN sys.dm_db_partition_stats ps on ps.object_id = i.object_id and ps.index_id = i.index_id
	GROUP BY
		t.name,
		i.name,
		i.type_desc,
		i.is_primary_key,
		is_unique_constraint
) t1
ORDER BY
    pages desc
;
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
