# First Responder Kit - Brent Ozar

> If you run `sp_Blitz` and you do what the high priorities say... And then you run `sp_BlitzFirst` to find out why performance is bad, read the URL advice and follow the instructions... You won't need me - Brent Ozar

https://training.brentozar.com/p/how-i-use-the-first-responder-kit

# Instructions

[FULL GUIDE HERE](https://github.com/BrentOzarULTD/SQL-Server-First-Responder-Kit)

Training class on how to use this stuff:
https://www.brentozar.com/product/how-i-use-the-first-responder-kit/

# Download

Page:
https://www.BrentOzar.com/first-aid

Direct download: https://downloads.brentozar.com/FirstResponderKit.zip

# Install

For SQL Server, to install all of the scripts at once:

1. Open `Install-All-Scripts.sql` in SSMS.
2. Switch to the database where you want to install the procs.
3. Run the query.

It will install the stored procedures if they don't already exist, or update them to the current version if they do exist.

# Overview

| Procedure                                                      | Description                                            |
| -------------------------------------------------------------- | ------------------------------------------------------ |
| [sp_Blitz](https://www.brentozar.com/blitz/)                   | Is my SQL Server healthy, or sick?                     |
| [sp_BlitzFirst](https://www.brentozar.com/askbrent/)           | Why is my SQL Server slow right now?                   |
| [sp_BlitzCache](https://www.brentozar.com/blitzcache/)         | Which queries have been using the most resources?      |
| [sp_BlitzWho](https://www.brentozar.com/first-aid/sp_blitzwho) | Which queries are running right now?                   |
| [sp_WhoIsActive](https://github.com/amachanic/sp_whoisactive)  | Which queries are running right now?                   |
| [sp_BlitzIndex](https://www.brentozar.com/blitzindex/)         | How could I tune indexes to make this database faster? |
| sp_BlitzLock                                                   | What queries and tables are involved in deadlocks?     |

You can see the available `@parameters` inside `Programmability / Stored Procedures`.

### Tutorials

-   [How I Use the First Responder Kit (FREE)](https://training.brentozar.com/p/how-i-use-the-first-responder-kit)
-   [Fundamentals of Index Tuning (PAID)](https://training.brentozar.com/p/fundamentals-of-index-tuning)
-   [Fundamentals of Query Tuning (PAID)](https://training.brentozar.com/p/fundamentals-of-query-tuning)

### Examples

```sql
-- Health check
sp_Blitz @CheckServerInfo = 1;

-- Top wait stats
sp_BlitzFirst @SinceStartup = 1;

-- Top 10 queries causing those wait stats in the past 24 hours
sp_BlitzCache @SortOrder = 'avg duration', @MinutesBack = 1440;
sp_BlitzCache @SortOrder = 'query hash', @MinutesBack = 1440;;

-- Indexes for a table
EXEC sp_BlitzIndex @DatabaseName='DATABASE_NAME', @TableName='TABLE_NAME';
```

# `sp_Blitz` - Is my SQL Server healthy, or sick?

https://www.brentozar.com/blitz

Overall health check. Is the server configured optimally.

Does all kinds of checks, and orders them by priority. Level 1-50 are fireable offenses.

> DBAs don't get fired because a server is slow. They get fired if a server is down, and they can bring it back.

Use when:

-   Taking over a new SQL Server.
-   Before signing off that a server is read for production.
-   When you come back from vacation.
-   DON'T use on a scheduled basis.

Show extra data under `Server Info`.

```
sp_Blitz @CheckServerInfo = 1
```

### CHECKDB

Check for data corruption.

It's essential to run CHECKDB on a regular basis to find errors if they exist because if there's corruption, we need to fix it before our last good backups disappear.

```sql
-- Look for dbi_dbccLastKnownGood for the time it last ran.
DBCC DBINFO('StackOverflow') WITH TABLERESULTS;

-- The corruption check
DBCC CHECKDB('StackOverflow') WITH NO_INFOMSGS, ALL_ERRORMSGS;
-- 40 min execution time
```

# `sp_BlitzFirst` - Why is my SQL Server slow right now?

https://www.brentozar.com/askbrent

Performance health check. Find the top server bottlenecks.

Check for performance issues as of **right now**, it helps with **everything is on fire** problems.

It takes a snapshot, and then another one after 5 seconds, and it compares the two. You can increase the gap as shown below.

```sql
sp_BlitzFirst @ExpertMode = 1, @Seconds = 60
```

A great way to use this is to set an Agent Job that runs the check every 15 minutes. Use this setting:

```
EXEC dbo.sp_BlitzFirst
@OutputDatabaseName = 'DBAtools',
@OutputSchemaName = 'dbo',
@OutputTableName = 'BlitzFirst',
@OutputTableNameFileStats = 'BlitzFirst_FileStats',
@OutputTableNamePerfmonStats = 'BlitzFirst_PerfmonStats',
@OutputTableNameWaitStats = 'BlitzFirst_WaitStats',
@OutputTableNameBlitzCache = 'BlitzFirst_BlitzCache',
@OutputTableNameBlitzWho = 'BlitzFirst_BlitzWho'
```

You can then query the tables to check what happened in the past.

```sql
sp_BlitzFirst @SinceStartup = 1
```

Base the stats on the complete server uptime. Good when you don't have an Agent Job history.

### Wait types

Use the result of this to solve with `sp_BlitzCache`.

```
sp_BlitzFirst @sinceStartup = 1, @OutputType = 'Top10'
```

A quick wait types report (try to have at least 24 hours of hours sample/wait time). Solve the top wait times first for the most improvement. There are links explaining each one.

# `sp_BlitzCache` - Which queries have been using the most resources?

https://www.brentozar.com/blitzcache

Find the queries causing the top bottlenecks (wait types).

It shows the top 10 most resource-intensive queries.

IMPORTANT: Run it after finding your top wait times with `sp_BlitzFirst @sinceStartup = 1, @OutputType = 'Top10'`.

By default it shows the most expensive by CPU usage, but the problem might be of another wait type, so modify the `@SortOrder` parameter based on your top wait type.

Based on your top wait in `sp_BlitzFirst`, here's a decoder ring for the 6 most common wait types, and how you should use `sp_BlitzCache`'s `@SortOrder` parameter:

| wait type                      | sort         | description                                                                                                                            |
| ------------------------------ | ------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| CXPACKET, CXCONSUMER, LATCH_EX | CPU, READS   | Queries going parallel to read a lot of data or do a lot of CPU work.                                                                  |
| LCK%                           | DURATION     | Locking, so look for long-running queries, and look for the warning of "Long Running, Low CPU." That's probably a query being blocked. |
| PAGEIOLATCH                    | READS        | Reading data pages that aren't cached in RAM.                                                                                          |
| RESOURCE_SEMAPHORE             | MEMORY GRANT | Queries can't get enough workspace memory to start running.Not available in older versions of SQL.                                     |
| SOS_SCHEDULER_YIELD            | CPU          | CPU pressure.                                                                                                                          |
| WRITELOG, HADR_SYNC_COMMIT     | WRITES       | Writing to the transaction log for delete/update/insert (DUI) work.                                                                    |

```sql
sp_BlitzCache @SortOrder = 'READS'
```

It returns two results:

1. The top 10 queries by your sort order.
2. Explanations of the "Warnings" column.

IMPORTANT: Always check the bottom result first, and focus on priority 1 warnings. Read the URL explanations. Fix that first, or you're wasting time.

Look at the columns to the right for Execution, Total Reads, Total CPU etc.

A lot of times, over-night maintenance jobs will turn up as the most expensive queries. You can filter those out like this:

```sql
sp_BlitzCache @SortOrder = 'avg duration', @MinutesBack = 90 -- How many minutes since 8AM (start of work) have passed?
```

# `sp_BlitzWho` - Who’s running which queries right now?

https://www.brentozar.com/first-aid/sp_blitzwho

It lists currently runnig queries, sorted from longest running, to shortest.

Use when people are complaining that SQL Server is slow, or in emergencies.

`sp_WhoIsActive` is a similar script, used when the server is so slow, `sp_BlitzWho` doesn't respond.

```sql
sp_BlitzWho @ExpertMode = 1 -- Show extra columns like logical reads, CPU usage
```

# `sp_BlitzIndex` - How could I tune indexes to make this database faster?

https://www.brentozar.com/blitzindex

Analyzes design issues with indexes. This is not a quick-fix script.

> Index tuning is the fastest way to make queries go faster without buying hardware or spending time in development.

Indexing is as much an art as is a science, despite there being a lot of vague guidelines, and sometimes having to break them in order to get better performance.

`sp_BlitzIndex` is about analyzing your database's overall issues, understanding which indexes are just holding you back, and which indexes to add.

```sql
-- List of findings and suggestions.
sp_BlitzIndex

-- Inventory of existing indexes, good for copy/paste in Excel for offline analysis.
-- Has a @SortOrder parameter for things like rows, size, reads, writes, lock time...
sp_BlitzIndex @Mode = 2, @SortOrder = 'rows'

-- List of missing high value indexes (create these)
sp_BlitzIndex @Mode = 3
```

**[Use this custom query for filterable indexes](https://github.com/8483/notes/blob/master/topics/database/queriesIndexes.md#list-of-all-indexes-with-various-stats)**

### One table analysis

```sql
EXEC dbo.sp_BlitzIndex @DatabaseName='DATABASE_NAME', @SchemaName='dbo', @TableName='TABLE_NAME';
```

Results:

-   1st: Indexes we already have.
-   2nd: Desperately needed indexes.
-   3rd: Table structure.
-   4th: Foreign keys
-   5th: Table statistics. Histogram (how pages are batched) for query tuning.

### Tuning indexes

> **Before you do any tuning, you want the server to have been up for at least 1 business cycle i.e. a full month.**

This gives you a pretty good idea which indexes are used. If the business is the same all the time (like a stock exchange), a day is enough. Avoid longer than a month.

You can tune indexes with the D.E.A.T.H. method:

-   Dedupe (remove duplicates).
-   Eliminate overlaps and unused indexes.
-   Add desperately needed indexes.
-   Tune indexes for specific queries.
-   Heaps i.e. Clustered indexes.

### READS vs WRITES (Usage column)

The amount of times the index made the query go:

-   Reads = FASTER
-   Writes = SLOWER (because of the INSERT/UPDATE/DELETE to keep the index in sync)

> **Remove indexes that have 0 reads, or a low-read high-write ratio.**

Look for `Est. benefit per day` above 1,000,000 as these are worth solving. Lower than that does more harm than good because it's like creating an index for every single query i.e. it will take forever to update the needless indexes.

Ex. A finding of `Index Suggestion: High Value Missing Index` and a usage of `27,425 uses, Impact: 100%; Avg query cost: 810` says that this index should be created because it will speed up the query by 100%.

> **Aim for 5 indexes (or less) per table, with around 5 columns (or less) per index.**

If you have this query:

```sql
SELECT * FROM Phonebook WHERE LastName = 'Ozar';
```

And these indexes:

```
LastName, FirstName, MiddleName INCLUDE Address, PhoneNumber

LastName, PhoneNumber
```

You are better off with using just the first one.

The suggested index columns (undex definition) are **NOT** in the right order! This is your job to do! Also, the create index column does not take into account the order.

The column which has the smallest cardinality, i.e. The least distinctive values, should always be positioned as the left-most side in a composite index. Applies for the rest of the columns.

To find the cardinality, use `DISTINCT`.

```sql
select count(distinct(category_id)) from users;   -- 16
select count(distinct(email)) from users;         -- 138,279
```

`category_id` should come before `email` in this case.

Index keys: Determine the index order. Used in filtering, join conditions, grouping and ordering.

Include: Stored only in leaf level, not part of the key, does not affect ordering, used to cover queries by supplying additional fields without forcing lookups.

> **Rule: Put in columns the fields you filter, join, group, or sort by. Put in include the fields you only need returned in the SELECT list.**

Equality columns first, inequality next, order/grouping columns last. Order does not matter for includes: they are unordered payload at the leaf level.

When multiple equality columns exist, cardinality guides their sequence. Put the most selective (highest cardinality) equality column first, then the next most selective, and so on—unless query ordering/grouping requirements dictate a different sequence.

Cardinality is the count of distinct values in a column. High cardinality = many unique values (e.g., GUID, ID). Low cardinality = few distinct values (e.g., Boolean, gender). Selectivity derives from cardinality: higher cardinality generally means more selective predicates.

### Index tuning example workflow

Create a change script:

```sql
-- The DROP and CREATE statements are generated for copying in the sp_BlitzIndex report.

/* Merge these 3 indexes into 1:

    The CURRENT definitions of the 3 indexes.

    into this new one:
*/

CREATE INDEX DownVotes_DisplayName_LastAccessDate ON dbo.Users(DownVotes, DisplayName, LastAccessDate)
GO
DROP INDEX [Index_DownVotes] ON [StackOverflow].[dbo].[Users];
DROP INDEX [DownVotes] ON [StackOverflow].[dbo].[Users];
DROP INDEX [IX_DV_LAD_DN] ON [StackOverflow].[dbo].[Users];
GO

/* Undo script (if you make a mistake and want to revert):

    The CREATE queries for the ORIGINAL 3 indexes.
*/
```

# `sp_BlitzLock` - Which queries and tables are involved in deadlocks?

Analyzes recent deadlocks, and groups them by table, query, app, login...

SQL Servers deadlock monitor wakes up every 5 seconds, looks for this scenario, and when it finds it, it kills the query that's easiest to roll back, called the victim.

Run when:

-   When `sp_Blitz` warns you about a high number of deadlocks per day.
-   When users complain about deadlock errors.

```sql
sp_BlitzLock
```

Usage:

1. Run with no parameters.
2. Jump to the bottom result set.
3. Identify the 1-3 tables involved in most deadlocks and tune their indexes.
4. Identify the top 1-3 queries, tune them, make them more SARGable, make their transactions short and sweet, change their isolation level.

To avoid deadlocks, you want to **use the tables in the same order** everywhere. Otherwise, deadlocks happen like this (executed in sequence):

**LEFT SIDE**

```sql
BEGIN TRAN

-- Runs 1st
UPDATE dbo.Lefty
    SET Numbers = Numbers + 1;
GO

-- Runs 3rd -- BLOCKING HAPPENS HERE, NOT DEADLOCK!
UPDATE dbo.Righty
    SET Numbers = Numbers + 1;
GO
```

**RIGHT SIDE**

```sql
BEGIN TRAN

-- Runs 2nd
UPDATE dbo.Righty
    SET Numbers = Numbers + 1;
GO

-- Runs 4th - DEADLOCK HAPPENS HERE!!!!!!
UPDATE dbo.Lefty
    SET Numbers = Numbers + 1;
GO
```

### Explanation of Deadlock

#### Scenario:

-   **LEFT SIDE session**: starts first, updates `Lefty`, then `Righty`
-   **RIGHT SIDE session**: starts second, updates `Righty`, then `Lefty`

#### Timeline:

1. **LEFT SIDE begins transaction**

    - Locks rows in `Lefty` table (`UPDATE dbo.Lefty`)

2. **RIGHT SIDE begins transaction**

    - Locks rows in `Righty` table (`UPDATE dbo.Righty`)

3. **LEFT SIDE tries to update `Righty`**

    - Blocked, because `RIGHT SIDE` holds lock on `Righty`

4. **RIGHT SIDE tries to update `Lefty`**
    - Blocked, because `LEFT SIDE` holds lock on `Lefty`

#### Result:

-   LEFT SIDE waits for `Righty` (locked by RIGHT SIDE)
-   RIGHT SIDE waits for `Lefty` (locked by LEFT SIDE)
-   **Circular wait condition met → deadlock**

SQL Server detects the cycle and terminates one session to break the deadlock.

# Identifying slow queries in MySQL

1. MySQL Slow query log. Enable it.
2. Use `processlist`.

```sql
show processlist;
show full processlist;
```
