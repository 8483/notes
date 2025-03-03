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
