# Execution order

![Execution order](../../pics/database/database_sql_execution_order.jpg)

# Design

Relational databases are designed to work with large tables, not with large numbers of tables.  
https://stackoverflow.com/a/53816164/3878760

With proper table structures and indexing, MySQL will comfortably handle millions of rows in any table. The "Table-per-?" model is never a Good Idea and invariably comes back to haunt you (usually when you need to summarise across all those tables)  
https://dba.stackexchange.com/a/264965

In general, it is better to store all rows in a single table rather than in multiple tables. To speed queries, you should use facilities such as indexes and partitions. What I would say instead is that storing entities across multiple tables just makes managing the database trickier, so why bother? I would recommend that you store such data in a single table, perhaps partitioned by month.  
https://stackoverflow.com/a/66614149/3878760

# Efficient queries

Databases are rarely CPU bound. Their 'compute' activity is generally very low. That being said, each case is different - and it is feasible that you are the exception rather that the rule. CPU _cores_ make a difference though. Specifically, in relation to how many open connections you allow to the DB simultaneously. So if you have an 8-core CPU, and you allow 8 simultaneous connections to the DB - each one will get access to a whole core on their own. If you allow 32 simultaneous connections, each one will get a 1/4 share of a core. So more cores gives you the ability to run multiple queries simultaneously.

Why would that matter, you may ask? Because a typical modern application will usually create a pool of connections to the DB. Then, when an incoming request for data is made, it will queue up for access to the connection pool. Once it is granted access - only then will it be able to execute the query and fetch data. What if you have 2 connections in your pool. But the first query in your application is a very long query (like some sort of ad-hoc report). Then the next query goes to connection-2, and all is well - it moves along. BUT, suppose the next query is also a long query (another ad-hoc report), so it binds up time on connection-2 (connection-1 is still working on the first report). Now _all_ of your connections to the DB are busy on those 2 reports. Nothing else gets done until one of those reports finishes (or times out). The more simultaneous connections you allow, the less likely you are to run into this scenario.

OK, so now you understand the value of 'more CPU' (not speed, but cores). What about more memory (faster memory would be almost irrelevant - assuming a modern bare minimum speed and not something from the 1980s)? When you fire up a query, a lot of the work for a modern DB is done "in memory", so you need sufficient memory that each of your simultaneous connections has plenty in order to complete the queries it will be running. Too little memory will have a compound effect of slowing down your queries and backing up your connections (due to slower queries). If you have significantly too little memory, some queries will not even run.

Storage I/O (faster storage) can also make a huge difference in DB performance. An SSD based DB will be significantly faster than a 'spinny disc' DB in nearly all cases, but especially READ performance. There are ways to mitigate this difference (RAID with redundant discs), but even with the mitigations, SSDs will perform better almost always. That being said, if everything else is waaay underpowered, even the best disc I/O will be nearly unnoticeable. The DB is a 'system' and the whole system is only as fast as the slowest link.

Another concern is Network I/O. I've seen a lot of DB systems where the application lives on app servers and the DB is a separate host that is shared across several different app servers. Imagine a case of a busy DB with 10 app servers hitting it. All 10 app servers have 10Gbps network interfaces, and the DB also has a 10Gbps network interface. Now, if each app server queries the DB and sucks down a large BLOB data record - you are going to saturate your DB server's network interface trying to deliver BLOBs of data to 10 different app servers.

The last point I want to make - nearly all DB performance issues are the result of poor DB normalization and query generation - and not related (explicitly) to the hardware running the DB. I can fire up a DB on the biggest, baddest, fastest super computer and and write a terrible query that will make it slow. I can run the same DB on a laptop and write an efficient query that will be blazing fast - and return the same data. Often times, people looking to 'improve DB performance by buying hardware' are really looking to compensate for badly designed table relationships and queries. The most significant thing in making most SQL DBs are indices. A good index can take a 20 second query and "fix" it to 10ms. And setting up indices isn't that hard. The problem is, indices take a lot of extra disc space, and require regular maintenance. Oh, and they aren't a 'magic wand' that will fix a poorly normalized DB, or resolve a badly structured query.

Now, for an example from my career. I was a Director of Professional Services for a team that built large websites for our clients. I was called out to help one of our teams that was experiencing severe performance issues on one of our client's builds. When I got there, I spent some time working with the team to see the symptoms they were experiencing. They were pointing out that the issue didn't appear in their development environment, but as soon as they tried to fire it up in production, the system would slow to a crawl and everything stopped working. I took a look at some of their code and saw a snippet where they did a SQL request to get a list of customers. Then, in a separate code fragment, they would take the results of the first query, and in a "loop" do a subsequent series of queries to get data for each customer found in the first query. I immediately understood the issue: in their development environment, they had ~10 customers in the dev database. But in production, they had 100s of millions of customers (this was a large web platform). Their first query, in development, returned a maximum of 8–10 records, which then bloomed into several dozen follow-on queries in the 'while' loop. But in production, their first query returned several thousand customer records, which bloomed into 100s of thousands of follow-on queries. I asked them why they didn't just get all the data they needed from the first query (using a join in SQL), and they said: "we didn't know how to do that"… Changing that code logic fixed the issue.

Likewise, I've seen many cases where the developer lazily does a "select _ from …" pattern in their code. The '_' means - bring me all the columns for this query. Then, in the code, they only use a small subset of the returned columns to perform the code operation. This lazy effort causes extra work for the DB. First, there's a lot of extra disc I/O from fetching the columns that aren't used. Second, that data has to be transmitted from the DB to the app server over the network - wasting network bandwidth (and time). Lastly, the data is ingested (usually in memory) by the app server, taking up a larger than needed memory footprint for the app server. All this for data that is essentially discarded after it is fetched from the DB. It's like packing all of your trash when you move to a different home. What a waste… It is far better to specify exactly which columns your code will need in the returned data - and then you save all the wasted space/effort/bandwidth.

Lastly, if you are having performance issues with a DB - checking the "slow query logs" is a good place to start. See which queries show up there the most (or take the longest times) and work on fixing those one at a time. Review the log and build a list of the worst offenders. Fix the 'easiest to fix' out of the top-10 first. Then the next easiest, etc. until you have cleaned your system up. There isn't a silver bullet that gets you out of a bad design, but there is a path to fixing it that will help…

Robin Wilson

https://www.quora.com/To-improve-the-performance-in-a-SQL-server-is-it-better-to-have-a-faster-CPU-faster-memory-just-more-memory-or-faster-storage

# How to Think Like the Engine - Brent Ozar

https://training.brentozar.com/p/how-to-think-like-the-engine

SQL Server works with 8KB pages i.e. chunks of data, kinda like excel sheets. 1 page = 1 logical read.

To show how many 8K pages the executed query read, turn on the statistics with the command below.

```sql
set statistics io, time on;
```

When you select data, it starts going through each page and does work.

Number of columns or rows returned DOES NOT affect speed. Speed mostly comes down to the number of pages needed to be read, less = faster. CPU and Memory are less important for speed.

> Clustered Index = Pages ordered by the primary key i.e. ID.

Index = A literal copy of a table, but sorted by a specific column.

Small indexes help a small number of queries. Queries usually need more than 1-2 columns. Large indexes also are bad, because they slow everything down due to IO processes i.e. updates become slower.

If the only copy of the data is the Clustered Index, and you are not returning one specific ID, you are doing FULL TABLE SCANS.

SQL Server DOES NOT cache query results. It caches pages. If 5 people run the same query, it runs 5 separate times.

ORDER BY is VERY expensive, around 4 times than a regular select.

> Non-Clustered Index = A copy of a table with sorted data ahead of time. It only stores the necessary columns, so it's smaller.

RULE: Do not have an order by, if you don't have a top??? Research this.

RULE: Don't index hot columns i.e. columns that are updated regularly.

RULE: You want as fex indexes as possible that support your workload. No indexes, every select scans tables. Too many indexes, Delete/Update/Insert queries do too many writes to update the indexes. As a general rule, for each table, aim for less than 5 indexes total, with 5 columns or less per index.

Index SEEK means jump to one area and start reading. Index SCAN means start at either end of the index. Neither seek nor scan refers to how much data we'll read.

It's totally possible to have a slow seek and fast scan depending on the data you are looking for.

WHERE without a supporting index = table scan.

ORDER BY without a supporting index = CPU work to rebuild the pages.

> Key lookup = Two index approach where you get the primary keys from an index, and then you look-up the rest of the data from the original table.

Hoverin on key lookup in the execution plan shows which columns had to be fetched (Output List). Adding these to an index will prevent this and improve speed.

Be aware that key lookups don't happen once, but happend for EACH ID (imagine it's a stack of executions in the execution plan). You can check how many time it executed by hovering over it.

RULE: Execution plans are read from right to left, top to bottom.

SQL Server uses statistics to predict how much data it will process, and it then chooses the execution plan that has the lowest cost (query bucks, an arbitrary grading by Microsoft)

Query optimization:

-   Read the execution plan left to right, top to bottom, and look for the first place where estimates vs actual are very different and work on that.
-   Key lookup is chosen when the index doesn't have enough columns (so data must be retrieved). Add those columns to an index to prevent fetching and improve speed.
-   When creating wider indexes, make sure to just get the right columns on the page, and make sure that the first column is something that you are searching for. Aim for 5 indexes per table, each with 5 columns.

Creating an index. The primary key (Id) is always included by default, so you can leave it out. The first column determines the order.

This query...

```sql
SELECT Id, DisplayName, Location FROM dbo.Users
WHERE LastAccessDate >= '202-01-01'
    AND LastAccessDate < '2012-02-01'
ORDER BY LastAccessDate;
```

Is drastically improved by this index. The first column is the one the index is ordered by, so the WHERE and ORDER are much more efficient.

```sql
CREATE INDEX IX_LastAccessDate_Id_DisplayName_Location
on dbo.Users(LastAccessDate, Id, DisplayName, Location)
```
