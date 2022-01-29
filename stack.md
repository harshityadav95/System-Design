# Database

## 1. Databases Store Transactions — Not State (Kind Of) <a href="#a54d" id="a54d"></a>

Databases, or rather their internal architecture, aren’t very intuitive. You might think that a database is simply a data file or two and some code that manages connections and modifications to that data file. And you would be right to a certain extent. But really, at its core, a database is just a log file. This log file is a list of transactions that have been submitted to the database kept in the order in which they were submitted.

Everything else (the state of your tables, rows, schemas, etc.) is emergent from the accumulated changes recorded on this log. Each database engine stores this log a different way, but a section pseudo-log might look something like this:

```
...
2021-06-07 12:24:35.044513-4000 INSERT INTO 'People' "jeff" "wilson"
2021-06-07 12:25:33.232098-4000 INSERT INTO 'People' "mike" "lou"
2021-06-07 12:25:37.140013-4000 INSERT INTO 'People' "pat" "cat"
2021-06-07 12:26:11.030002-4000 INSERT INTO 'People' "mark" "wilson"
...
```

From this log, a database can rebuild its tables, databases, schemas, and everything else if the data files are somehow corrupted. But there’s one very important caveat: Once a transaction is committed or rolled back, it is removed from the log. The purpose of the log is to stage changes until a transaction is completed — not to act as a sort of backup mechanism. Somewhat small and insignificant blips in the database can be recovered from the log, but anything more serious than a blip will call for some kind of external backup to be restored.

In PostgreSQL and some other relational databases, this log is called the Write-Ahead-Log (WAL). Managing this WAL and its various features is a big part of performance-tuning these databases and also how PostgreSQL manages replication. Any transaction that is written to the WAL is also broadcast to its replication peers so they can add the transaction to their WAL.

Understanding this mechanism is key to understanding how databases work and troubleshooting them when things go bad.

## 2. Choosing the Right Database Is Hard <a href="#ffeb" id="ffeb"></a>

I’ve seen a lot of dogmatic fist-banging about “the best” or “the worst” database, but the truth is the best database is the one that works best for your application. There’s no one-size-fits-all sort of database just like there’s no one-size-fits-all programming language or operating system.

When starting a new project, choosing the right database can be one of the most crucial decisions that you’ll make. So how should you choose which DB to use? I put together a [list of five things to consider in my article on databases for developers](https://betterprogramming.pub/db-101-databases-for-developers-f7dc29f46f2c), but let me also quickly go through them here.

### What kind of data will be stored in the database? <a href="#12bf" id="12bf"></a>

Are you storing log files or user accounts?

### How complex is the data that will be stored? <a href="#7018" id="7018"></a>

Can the data be normalized easily?

### How uniform is the data? <a href="#60ce" id="60ce"></a>

Does your data roughly follow the same schema or is it disparate or heavily nested?

### How often will it need to be read or written? <a href="#5b3c" id="5b3c"></a>

Is your application read- or write-heavy, or both?

### Are there environmental or business considerations? <a href="#9e75" id="9e75"></a>

Do we have existing agreements with vendors? Do I need vendor support?

By answering these questions, you can help narrow down your choices to a few candidates. Once there, testing should tell you which one is the best for your application.

## 3. Moving to the Right Database Is Harder <a href="#8886" id="8886"></a>

Sometimes you don’t have a choice and the database is already chosen for you. Whether you came to the project after it was started or political winds forced you a certain way, using the wrong database for the job can be frustrating.

But equally, if not more, frustrating is the progress of migrating databases should you get the opportunity. Once you start down one path, it's not easy to simply change paths in the middle of things. Not only do you have to figure out a way to replicate your data from one database to another and learn a whole new system, but depending on how tightly coupled your database code is with the rest of your application, you might also be looking at extensive rewrites as well. Changing databases is not a task that should be undertaken lightly and without a lot of consideration, debate, testing, and planning. There are so many ways that things can go horribly wrong. This is why #2 is so important: Once you choose, it's hard to undo that choice.

## 4. NoSQL Doesn’t Replace SQL, It Complements It <a href="#4d5c" id="4d5c"></a>

The debate about using a SQL or NoSQL database will go on forever. I get that. But often missed in this argument is the fact that NoSQL databases don’t _replace_ SQL databases. They _complement_ them.

There are some things that NoSQL databases do very well and there are some things that SQL databases do very well. Prometheus is very good at storing time-series data like metrics, but you wouldn’t want to use MySQL for that. Is it technically possible? Yes, but it's not designed for that and you’re not going to get the best performance or developer experience out of it. On the flip side, you wouldn’t want to use Redis to store highly relational data like user accounts or financial transactions for the same reasons. Sure, you could make it work in the code, but why add that complexity and headache when you could just use the right tool for the job?

There is going to be some inevitable overlap in some areas. There are some excellent databases that are _technically_ NoSQL that do a good job of storing relational data (see: Couchbase), but there are other outside factors that go into using one over the other. Factors like client language support, operational tooling, cloud support, and others are all things to take into account when choosing a database.

## 5. Scaling Is H_ard_ <a href="#6cac" id="6cac"></a>

Databases present a unique challenge when it comes to trying to scale them. Because they store state and are therefore inherently stateful applications, finding ways to replicate that state across multiple database instances in a way that is consistent, safe, and fast enough to be transparent to any application is _hard_. This is why the most common way of scaling databases — especially relational databases — is with _vertical scaling_.

Let’s pause and take a minute to talk about the two kinds of scale: vertical and horizontal. To understand each method properly would take an entire article of its own (not a bad idea, actually…), but for now, we can break this down fairly simply. I think the best way I can explain this is with an analogy, so here goes: Let's say that you have a one-gallon jug of water, but it's got a small hole in it, so you need to move the water to another container. However, all you have are smaller containers that are less than one gallon. You could use two methods:

1. Use a high number of smaller containers (like drinking glasses) and distribute the gallon of water across those many containers.
2. Use a low number of one-quart containers and distribute the water over those few larger ones.

Using many smaller containers is _horizontal scaling,_ where the workload (or water) is spread across many smaller containers. Using a few larger containers is _vertical scaling_, where the load is spread across a small number of large containers. With horizontal scaling, when you need to add additional capacity, you add more containers. With vertical scaling, you increase the few containers you already have.

With databases, the common practice is to vertically scale your database instances by adding CPU and RAM capacity. This avoids the issue of having to replicate state across a high number of instances but still allows your database to take on the extra load.

Some databases — especially NoSQL databases — can be massively horizontally scaled, but they usually come with a trade-off in consistency. These databases tend to be _eventually consistent,_ where the data will _eventually_ become consistent across the entire cluster, but replication may not be transparent to any applications accessing that data. That is, an application reading from database node A will get one version of the data, where an application reading from node D will get another version until the cluster can update that data on all nodes. While this might sound like a deal-breaker, if you design and build your applications with this in mind, it doesn’t have to be an issue.

## 6. Indexes Are Like Magic Until They’re Not <a href="#010f" id="010f"></a>

Indexes are arguably one of the most important aspects of databases, yet they are often one of the most overlooked. An index, simply put, is a table of contents for the database when looking up data. Instead of having to scan an entire column looking for a single value, an index can tell it where that value is so the database engine can jump to it immediately.

If you’re reading this and saying to yourself, “Hey, this sounds like a hash table,” well, you’re not wrong. Indexes are basically a type of hash table for a given column in a table. Most relational databases automatically create an index on the primary key, but you can add indexes to as many columns as you wish.

But please don’t do this.

While indexes can speed up your read times significantly — especially in large datasets — they come with a trade-off when writing data to a table. Every time the table is updated, the indexes on that table also need to be updated, which adds extra time to each write transaction. This is because of what an index actually _is_. Unlike a book’s table of contents that is simply a list of pointers, an index actually contains a second copy of the necessary columns just stored in a different order. This means that indexes use a proportional amount of disk space and require I/O when being updated. When dealing with only a few indexes on a table, the trade-off is usually negligible, but any more than a few and your transactions start incurring the write penalty of having to update all of those indexes on every transaction.

This is why it is important to be strategic about where and when you use an index. The best way to decide whether or not to index a column is to take a hard look at your application, see what kinds of queries are most often used, and base your decision on that. Also, tools like application performance monitoring or database monitoring can help unearth any queries that might be improved with the addition of an index by tracking transaction times for the various database queries.

## 7. Transactions <a href="#7ff2" id="7ff2"></a>

Every time something interacts with a database, it’s using a construct called a _transaction_. In the database world, a transaction is an atomic unit of work — a standalone, discrete action to take that has one of two results: commit or roll back.

This might not sound like a big deal, but it has very real implications for the performance and operation of your database — especially for those that are[ ACID-compliant](https://en.wikipedia.org/wiki/ACID). The order in which transactions are submitted and the amount of data processed in those transactions can have a huge impact depending on the transaction isolation configured in the database. A friend of mine relayed this little anecdote about one experience with transaction blocking:

> “Years ago, someone was annoyed that the ‘database was slow and timing out,’ and it turned out that a team member had arranged to work from Costa Rica, on a slow internet connection, and the client application they had installed on their laptop did a `SELECT *` from a table used by everyone, so every time it ran, the database would lock the entire table for the duration of time it took to send all the results back to Costa Rica since even a `select` is a transaction.”

This is because the database server had to guarantee the consistency of the data when the client receives the data back from the server, so no other transactions could be processed in the time that it took to run the query and transmit the data back to the client. On a slow connection, with a lot of data, or both, this can cause some major bottlenecks for anyone else using the database.

Some of these limitations can be overcome by tuning your requests to only fetch the data you need, using read-replicas that sit close to the users or applications that will be using your database, tuning your transaction settings to fit your needs, or all of the above.

* Extendable workflow automation( [https://n8n.io/](https://n8n.io) )



Testing &#x20;

* Open Source Load Testing tool Locust ([https://locust.io/](https://locust.io))
