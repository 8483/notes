# List of all indexes with various stats

```sql
with
	indexes as (
		SELECT
			case
				when user_scans > user_seeks then '1. delete - scans'
				when user_updates > user_seeks then '1. delete - writes'
				when CAST(user_scans AS float)  / nullif(user_seeks + user_scans, 0) > 0.3 then '2. improve'
				when frag.avg_fragmentation_in_percent > 30 then '4. rebuild - defragment'
				when i.fill_factor < 100 and isnull(round(CAST(user_updates AS float)  / nullif(user_seeks + user_updates, 0) * 100, 0), 0) < 10 then '4. rebuild - fill factor 100'
				else ''
			end status,

			t.name table_name,
			i.name index_name,

			user_seeks seeks,

			user_scans scans,
			isnull(round(CAST(user_scans AS float)  / nullif(user_seeks + user_scans, 0) * 100, 0), 0) SCvSK,

			user_lookups lookups,

			user_updates writes,
			isnull(round(CAST(user_updates AS float)  / nullif(user_seeks + user_updates, 0) * 100, 0), 0) WRvSK,

			i.fill_factor fill,

			cols,
			keys,
			isnull(includes, '') includes,

			isnull(used_page_count * 8 / 1024, 0) size_mb,
			isnull(used_page_count, 0) pages,
			round(frag.avg_fragmentation_in_percent, 0) frag_prcnt,

			last_user_seek,
			last_user_scan,
			last_user_lookup,
			last_user_update,

			/*
			fk.object_id,
			i.type_desc index_type,
			i.is_primary_key,
			i.is_unique_constraint,
			*/

			o.create_date created,
			o.modify_date modified,

			concat('ALTER INDEX [', i.name, '] ON [', t.name, '] REBUILD WITH (FILLFACTOR = 80);') rebuild_command,
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
						CAST(CASE WHEN ic.is_included_column = 0 THEN c.name END AS VARCHAR(MAX)), ', '
					) WITHIN GROUP (ORDER BY ic.is_included_column, ic.key_ordinal, ic.index_column_id) keys,
					STRING_AGG(
						CAST(CASE WHEN ic.is_included_column = 1 THEN c.name END AS VARCHAR(MAX)), ', '
					) WITHIN GROUP (ORDER BY ic.is_included_column, ic.key_ordinal, ic.index_column_id) includes
				from sys.index_columns ic
					join sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
				group by
					ic.object_id,
					ic.index_id
			) c on c.object_id = i.object_id AND c.index_id = i.index_id

			JOIN sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') frag on frag.object_id = i.object_id and frag.index_id = i.index_id and fragment_count is not null
			JOIN sys.dm_db_partition_stats ps on ps.object_id = i.object_id and ps.index_id = i.index_id
			JOIN sys.dm_db_index_usage_stats st on st.object_id = i.object_id and st.index_id = i.index_id
			LEFT JOIN sys.foreign_keys fk ON fk.referenced_object_id = i.object_id AND fk.key_index_id = i.index_id
		where
			i.type_desc = 'NONCLUSTERED' -- Actual indexes. CLUSTERED = tables
			and fk.object_id is null -- Remove indexes created automatically from foreign key constraints
	)
select *
from indexes
order by
	sum(seeks) over (partition by table_name) desc, -- running total
	seeks desc
;
```

# List of missing/desired indexes

```sql
select *
from (
    SELECT
        round(migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans), 0) AS [index_advantage],
        migs.avg_total_user_cost,
        migs.avg_user_impact,
        migs.user_seeks,
        migs.user_scans,
        mid.statement AS [table],
        mid.equality_columns,
        mid.inequality_columns,
        mid.included_columns,
        migs.last_user_seek
    FROM sys.dm_db_missing_index_groups AS mig
        JOIN sys.dm_db_missing_index_group_stats AS migs ON migs.group_handle = mig.index_group_handle
        JOIN sys.dm_db_missing_index_details AS mid ON mig.index_handle = mid.index_handle
) t1
ORDER BY
    sum(index_advantage) over (partition by [table]) desc,
    index_advantage DESC
;
```

# Column importance for missing indexes

```sql
WITH MissingIndexes AS (
    SELECT
        mid.statement AS [table],
        mid.equality_columns,
        mid.inequality_columns,
        mid.included_columns,
        migs.user_seeks,
        ROUND(migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans), 0) AS index_advantage
    FROM sys.dm_db_missing_index_groups AS mig
    JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
),
sums as (
    select
        [table],
        sum(user_seeks) user_seeksTotal,
        max(user_seeks) user_seeksMax,
        sum(index_advantage) index_advantageTotal,
        max(index_advantage) index_advantageMax
    from MissingIndexes
    group by [table]
),
Unpivoted AS (
    SELECT [table], LTRIM(RTRIM(value)) AS col
    FROM MissingIndexes
    CROSS APPLY STRING_SPLIT(COALESCE(equality_columns, ''), ',')
    UNION ALL
    SELECT [table], LTRIM(RTRIM(value)) AS col
    FROM MissingIndexes
    CROSS APPLY STRING_SPLIT(COALESCE(inequality_columns, ''), ',')
    UNION ALL
    SELECT [table], LTRIM(RTRIM(value)) AS col
    FROM MissingIndexes
    CROSS APPLY STRING_SPLIT(COALESCE(included_columns, ''), ',')
),
Counts AS (
    SELECT [table], col, COUNT(*) AS occurrences
    FROM Unpivoted
    WHERE col <> ''
    GROUP BY [table], col
)
SELECT
    c.[table],
    STRING_AGG(CONCAT(col, ' (', occurrences, ')'), ', ')
        WITHIN GROUP (ORDER BY occurrences DESC, col) AS column_counts,
    max(s.user_seeksTotal) user_seeksTotal,
    max(s.user_seeksMax) user_seeksMax,
    max(s.index_advantageTotal) index_advantageTotal,
    max(s.index_advantageMax) index_advantageMax
FROM Counts c
    join sums s on s.[table] = c.[table]
GROUP BY c.[table]
ORDER BY index_advantageTotal desc
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
