# Execution order

![Execution order](../../pics/database/database_sql_execution_order.jpg)

# CPU vs RAM

> If your RAM is less than your total sum of databases size, add RAM first before adding CPUs.

Increase RAM so at least frequently used indexes can entirely fit into RAM.Ex. 32GB RAM is plenty for a 350GB database, because **indexes are what you need in RAM, not raw data**)

No amount of extra cores will match the performance benefits of having all of your DBs and ancillary data (indexes etc) in memory. Extra cores will be useful if you have lots of concurrent clients hitting your or you do a lot of background work such as reporting or indexing.

Order of importance:

1. **Disk subsystem speed.** RAID10 in my experience is best. Bonus points for SSDs.
2. **Total amount of RAM.** The more RAM, the more cache your server will be able to have.
3. **Memory speed.** Faster RAM is obviously better than slower RAM, however RAM is always faster than disks, so more slower RAM is better than less faster RAM.
4. **Number of CPU cores.**
5. **CPU speed.**

This obviously depends on the application, but typically a database server's job is to provide really fast access to data, so the CPU speed is less important than the speed of access to the data (disks and RAM). But obviously if you're using a lot of math / calculations in your queries, you need more CPU resources.

The general advice is to ensure you have enough RAM to hold your working set of data before adding more CPU resources.

SQL engines will consume as much memory as you will allow.

The reason for this is that the engine caches the data in RAM, so that it can access if faster than it could if it needed to read the data from the disk every time a user needed it.

Memory (RAM) Usage: Databases use RAM to store frequently accessed data, indexes, and cache query results. If your RAM is less than the total size of your databases, it means not all frequently accessed data can be stored in memory, leading to more frequent disk I/O operations which are slower compared to accessing data in memory.

CPU Usage: While having more CPUs can improve performance, particularly for complex queries and parallel processing, the benefits will be limited if the system is frequently waiting on disk I/O due to insufficient RAM.

By adding more RAM, you can reduce the need for disk I/O, thus speeding up database operations. Once you have sufficient RAM to handle your workload and reduce disk I/O, adding more CPUs can then further enhance performance, particularly for handling concurrent queries and complex computations.

# RAM

-   Logical Reads: Access data in memory; fast; preferred for performance.
-   Physical Reads: Access data on disk; slow; to be minimized.

**Key Points to Consider**

-   **Buffer Pool Usage (45GB)**
    -   What It Represents: This indicates the current amount of memory used by the database to cache data pages and indexes in the buffer pool. It's a reflection of the active workload and what the database engine is currently keeping in memory to optimize performance.
    -   Importance: Ensuring that your buffer pool can hold your working set (frequently accessed data) is critical for performance. This helps minimize disk I/O operations.
-   **Index Sizes (117GB)**
    -   What It Represents: This is the total size of all indexes in your database. It includes all index pages that might be needed for query execution.
    -   Importance: Indexes play a crucial role in speeding up query performance. If your indexes do not fit into memory, the database will need to perform disk I/O to read index pages, which can slow down query execution.

**Recommendations**

1. **Prioritize Buffer Pool**
    - Reason: The buffer pool size is more indicative of the current active workload and the immediate memory needs of your database. If the buffer pool usage is 45GB, it means that your database currently requires 45GB to cache the frequently accessed data pages and indexes.
    - Action: Ensure that your RAM is at least equal to the buffer pool usage to accommodate the current working set efficiently.
2. Consider Index Sizes for Growth and Performance
    - Reason: While the total index size is 117GB, not all indexes might be used frequently. However, having sufficient RAM to accommodate a significant portion of your index data can improve query performance, especially for complex queries and those that involve large scans or lookups.
    - Action: If possible, provision additional RAM beyond the buffer pool size to accommodate a larger portion of your index data. This can help in handling peak loads and improving overall performance.

**Practical Approach**

-   Minimum RAM: Start with at least 45GB of RAM to match the current buffer pool usage.
-   Optimal RAM: Aim for additional RAM to cover a significant portion of your index data. A practical target could be somewhere between the buffer pool usage and the total index size, depending on your budget and performance requirements. For example, you might aim for 64GB or more.

# Design

Relational databases are designed to work with large tables, not with large numbers of tables.  
https://stackoverflow.com/a/53816164/3878760

With proper table structures and indexing, MySQL will comfortably handle millions of rows in any table. The "Table-per-?" model is never a Good Idea and invariably comes back to haunt you (usually when you need to summarise across all those tables)  
https://dba.stackexchange.com/a/264965

In general, it is better to store all rows in a single table rather than in multiple tables. To speed queries, you should use facilities such as indexes and partitions. What I would say instead is that storing entities across multiple tables just makes managing the database trickier, so why bother? I would recommend that you store such data in a single table, perhaps partitioned by month.  
https://stackoverflow.com/a/66614149/3878760

# Performance

-   Always foreign key to IDs, rather than column values.
-   Try to stick to `where` clauses on indexed columns, instead of `like`.
-   Don't go crazy with `joins`.
-   Don't use varchar(255). Try to use the lowest number possible.

# Execution Order

# Start with filtered data sets

# Use CTEs

# Use indexes

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
