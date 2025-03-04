```
I would go with Multi-Tenancy myself. "It will be an incredible mess having a shared database for everyone and differentiating data access with a clientId." Tying client identity into SQL queries is arguably a good thing anyway. I would say this mess would be significantly less incredible then the mess of trying to create distinct database servers, or even distinct databases. Schemas are strictly defined. If you normalize your data well, it shouldn't be confusing to access. As fro the performance, when you need to, you can do multi-tennant multi-dbserver sharding. Just keep data ownership in mind and make identity and access control a part of the query conditions, and if you have a good normalized schema it should come together really well.
```

https://stackoverflow.com/questions/77959215/subdomain-single-tenancy-database-per-tenant-vs-container-per-tenant

---

```
I'd avoid jumping too quickly into any solution that will rapidly increase your maintenance responsibilities.

I would reconsider multi tenancy. The biggest issue then will be ensuring that clients only ever see their own data, but by correctly using foreign keys and with a decent set of automated tests, you should be able to solve it.

With a good set of indexes, having millions of rows shouldn't be a problem.

To scale: At first: use a more powerful server Throughput: use group replication (should probably start with this anyway for data safety) After that: you could create multiple, separate databases and assign each client to one of them. Create a script to transfer a client from one to the other, which could be done with minimal downtime: copy data last updated before transfer start time, disable client, copy data updated since start time, update assigned group, enable client.

Indexes make such a huge difference, queries that would otherwise take minutes can be reduced down to milliseconds.

Why not do some tests? Take an instance and fill it with 3 years of dummy data from 1000 clients. Ensure you have indexes on everything that you select by. Use EXPLAIN to see what's happening with any remaining slow queries.

You're using mysql or postgresql right?

This will be less performant than other options, but not by much and comes with the advantage of far less maintenance.
```

https://www.reddit.com/r/webdev/comments/1aln15l/subdomain_singletenancy_database_per_tenant_vs/

---

```
You should prefer simpler solutions unless you have data that shows added complexity is worth it. Using 2 is a very simple solution and is a great place to start and from there you can test to see where your performance bottlenecks are. Once you know your bottlenecks you'll know which areas to work on and what architectures those imply.
```

```
I'd go with 2a as well. The other solutions won't scale as well. The only downside is that programming errors could potentially expose one customer's data to another.

That risk should be manageable by putting in proper access controls and writing solid automated tests for those access controls.
```

https://www.reddit.com/r/learnprogramming/comments/5rcx7x/singletenant_vs_multitenant_database_for_saas/

---

```
Actually one database with all clients, using VPD (Virtual Private Database) and EBR (Edition Based Redefinition). Oracle has both of those features. Your clients will never be able to see other client data and you’ll be able to migrate all clients to a new version of the application in the same database. EBR allows for two versions of the database (current version and next version) and you can have clients connected to both versions at the same time.

This addresses all of your concerns.

One single database for all tenants. One application schema that holds all the tables and other database objects for everyone. VPD will ensure that each tenant only sees their own data. EBR will allow you to do zero downtime migrations between two versions of your application.

If you haven’t started to build the front end yet, Oracle has two free solutions that will help you with the front end development and delivery Oracle APEX and Oracle ORDS.

A database or container per tenant is not a great design. A LOT more work and effort and scaling to a lot of tenants means a LOT more work. With the design I’m suggesting, a new tenant is a few more rows in a table. I can trivially do billions of rows in a table on my laptop.
```

```
Approach 1 or your bonus approach are the correct choice. Approach 1 (multiple databases) is going to have some unique challenges in that you have to make sure all the databases get e.g. the same schema updates, which can be non-trivial with a large number of databases. Some of the largest accounting companies in the world use a single shared database for multiple tenants like your describe in your bonus approach.

Approach 2 seems like it'd be a mess, and wasteful. If you have 5000 customers, that means you have 5000 containers. It's likely that only a handful of those actually have active users at any one time, so all the other containers are just sitting there doing nothing? And then if you want redundancy, you need to 3x your containers... so now for 5000 customers you have 15,000 containers...? That sounds like an absolute nightmare from a scalability/manage-ability perspective.
```

https://www.reddit.com/r/Database/comments/1aln0eq/subdomain_singletenancy_database_per_tenant_vs/

---

```
Who are your tenants and how sensitive is the data? Some clients will insist on full separation if they can't self-host (their data/systems do not comingle with others) while others will not. In my experience other decisions and re-architectures are small compared to this one.
```

```
I think the easiest way is to go the database way. And just link your data to a tenant id of some sort (this can be a composite, too)

And then you just have to make sure your queries always filter by this.
```

https://www.reddit.com/r/ExperiencedDevs/comments/tdpkrz/designing_multitenant_systems/

---

```
I've gone through this a decade ago. Our solution was to have a single large database with a tenantId column in relevant tables.

The issue that I encountered pretty quickly, as the number of tenants and users grew, was performance. There are several things that we did to get around this problem.

Partitioned the tables (this was SQL Server - it's a built in feature) by tenantId. This way hitting a query for a single tenant would have very little impact on other tenants. SQL Server made it pretty easy.

Changed some highly traveled tables to be In-Memory. Got a huge boost from this. It required purchase of more RAM. This step required some tinkering as In-Memory tables are not allowed to have foreign keys and other goodies that we take for granted.

Created a mirror DB server for read only queries. For instance, all report queries would go to the mirror. Made a huge difference as well since some of those reports were super hairy.

Our plan was to split the data into separate DB Servers if the above steps weren't enough. But they were, with a lot of headroom. For reference, the DB size was about 3TB.
```

```
Databases get exponentially more expensive the bigger they get. So scaling horizontally with db per client can be ideal.

Make the code work either way (multiple client per db and db per client ) then you can slice and dice per clients need.

Keep a bunch of small clients on one db and enterprise clients on their own.
```

```
A couple of the classic design principles apply here, I think:

1) Don't optimize prematurely -- make a single database work until it doesn't 2) Design modularly so that #1 isn't impossible to change

At my company we have something in the hundreds of terabytes of customer data across our system(s). We have PII, legal matters, medical information and have to obey all regulations regarding all of those (and more). And we run single DB for all tenants in a given shard. We split shards based on more than just DB performance -- when any part crosses a scaling threshold we start a new shard (or multiple). Sharding the database by itself isn't something we've felt is the right choice yet, and we have dozens of production shards.

Unless you have strict security requirements, then I would embrace simplicity. You'll go faster and make a better product in the long run as long as you plan well.
```

```
It really depends. Mostly on the number of tenants you expect to have, and how different or similar their needs will be.

If you're aiming (in the longer term) for many small tenants of reasonably similar size and needs, I'd stick to a single database. You'll save yourself a lot of work that way. Imagine having to upgrade a thousand databases. A DB migration fails on some of your 1000 databases... and now they're out of sync, and someone will need to (i) notice and (ii) fix them one by one. If you want to make the multi-db approach work for lots of small tenants, expect to invest heavily in automation and monitoring.

If you're aiming for fewer and larger tenants that you want or need to treat more individually and not so much in bulk, separate DBs per tenant might make sense. If for example you want to offer your tenants the ability to upgrade to new versions at a different pace, or to allow/require some of them to pay more for higher performance (or because one has massively more data than the others and they need a higher tier db server to avoid killing performance for the smaller ones sharing the same db).
```

```
Both have pros and cons but basically large clients get their own DB, small ones share.
```

```
I'd put all tenant data in one database, but your idea is totally fine too. That way there will be less databases to maintain. Imagine schema updates and data migrations for 100 different tenant databases if your software becomes successful. With just one database (or only a few), it's much less maintenance overhead.

You can at least have all the small tenants share the same database, but put the data of bigger tenants who pay more in separate databases.
```

## https://www.reddit.com/r/dotnet/comments/1acrx5r/good_multitenant_architecture_for_saas/

```
As someone who also has done multi tenancy with database per tenancy i would never ever do it again. It can get out of hand quite fast. I would definitely recommend it for small projects where you can have from 1 to maybe 600 clients at most but out of that? Just do modular monolithic application and use filters. Honestly it will be more maintainable and can scale easi. But again my domain was quite complex and had a lot of features.
```
