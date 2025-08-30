# List of all indexes with various stats

```sql
SELECT
	t.name table_name,
	i.name index_name,

	user_seeks seeks,
	user_scans scans,
	user_lookups lookups,
	user_updates updates,

	cols,
	columns,

	isnull(used_page_count * 8 / 1024, 0) size_mb,
	isnull(used_page_count, 0) pages,
	round(frag.avg_fragmentation_in_percent, 0) frag_prcnt,

	case when frag.avg_fragmentation_in_percent > 30 then 'yes' else 'no' end rebuild,

	o.create_date created,
	o.modify_date modified,

	last_user_seek,
	last_user_scan,
	last_user_lookup,
	last_user_update,

	i.type_desc index_type,
	i.is_primary_key,
	i.is_unique_constraint,

	concat('ALTER INDEX ', i.name, ' ON ', t.name, ' REBUILD WITH (FILLFACTOR = 80);') rebuild_command,
	concat('DROP INDEX [', i.name, '] ON [', t.name, '];') drop_command

FROM sys.indexes i
	JOIN sys.objects o on o.object_id = i.object_id
	JOIN sys.tables t ON i.object_id = t.object_id

	JOIN (
		select
			ic.object_id,
			ic.index_id,
			count(c.name) cols,
			STRING_AGG(
				CAST(
					CASE
						WHEN ic.is_included_column = 0 THEN c.name
						ELSE '+' + c.name
					END AS VARCHAR(MAX)
				), ', '
			) WITHIN GROUP (ORDER BY ic.is_included_column, ic.key_ordinal, ic.index_column_id) columns
		from sys.index_columns ic
			join sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
		group by
			ic.object_id,
			ic.index_id
	) c on c.object_id = i.object_id AND c.index_id = i.index_id

	JOIN sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') frag on frag.object_id = i.object_id and frag.index_id = i.index_id and fragment_count is not null
	JOIN sys.dm_db_partition_stats ps on ps.object_id = i.object_id and ps.index_id = i.index_id
	JOIN sys.dm_db_index_usage_stats st on st.object_id = i.object_id and st.index_id = i.index_id
ORDER BY
	sum(used_page_count) over (partition by t.name) desc, -- running total
	used_page_count desc
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
