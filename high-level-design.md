# High Level Design



### **HLD Questions** <a href="#1cb3" id="1cb3"></a>

![Image for post](https://miro.medium.com/max/60/0\*hD6tIy4NN8X-3-2Z?q=20)![Image for post](https://miro.medium.com/max/5184/0\*hD6tIy4NN8X-3-2Z)

All the questions asked in HLD are pretty vague. The interviewer can sometimes give constraints, or else you will need to drive them. You have to discuss with the interviewer and define the scope of the problem you are going to solve. I have put down the scope for the questions that the interviewer and I finalized on. It is common for interviewers to pick a few components and deep dive into it during the interview.

1. **Design a distributed locking system.**

* You cannot use any external database system. The only access you will have is to the local filesystem.
* Latency for checking or acquiring a lock should be less than 10ms.
* You are going to get an equal amount of read and write load.
* The application should be Highly Available.
* You should take care of the time drift.
* How are you going to provide resiliency without downtime.
* How will you architect it in a multi DC scenario.

**Note:** Though you are not going to use any external database system, you will end up discussing internals and protocols used by a lot of external databases like Cassandra, Zookeeper, etc. for solving it.

**2. Design a system to handle flash sale.**

* You are going to receive the traffic 100x of the BAU.
* Particular focus needs to be given to the product and checkout page and how we are going to handle high demand with limited inventory.
* The product page should be of low latency, high throughput, and high availability. The checkout page should support high throughput and high availability.
* The system should be as fair as possible. If I ordered the item before someone else, then I should be given priority.
* The product page should reflect if the item is no longer available.
* If the item is no longer available, I should be put into a fair wait queue, where I will get a chance to buy the product if someone cancels the order or new units are added to inventory.
* I should take care of the hotspots in my databases (for example -> details for the flash sale product is going to the same shard if the cache used is partitioned by product id)

**3. Design a tagging system.**

* People can put tags on their documents, and people are able to search based on those tags.
* The tag should reflect and be searchable for the person who added it in a transactional manner. The tag can be propagated for other user’s search results with a little lag or delay (async fashion).
* Provide the rest API contracts for your service.
* Make database type choices and create a database schema for your service.
* You need to serve a set of popular tags in a time window, for example, the most popular tags in the last seven days in real-time.
* How are you going to architect it in a Multi-DC environment.
* The build and deployment pipeline for it, metrics to push, alerts etc.

**4. Design a food delivery app**

* People should be able to search for the nearest restaurants. Search service should be highly available and low latency. Adding/Deleting a restaurant can be eventually consistent for the search view.
* One order will only be associated with one restaurant. A person can order multiple items and multiple quantities of the item.
* A delivery guy needs to be assigned for every order with a tracking service. The heuristics should be to assign the delivery guy who is nearest to the restaurant and is available.
* You need to design the database schema after the database choices for different read/write patterns and REST API Contracts.

**5. Design a dashboard for serving real-time insights (requires designing a data warehouse solution)**

* Ingest events happening across the organization. (These can be transactional events too and hence might need to relay the logs from DBs, apart from the services already pushing into the message bus) .
* How the schema registration and validation is going to happen for these events.
* In what type of data store and format should the data be stored for doing aggregates over this data in batch mode (Avro, Parquet, ORC, etc.)
* It might be required to join data between multiple streams for real-time insights. Data enrichment might be required from URL endpoints.
* How the raw events are going to be stored, and historic and reconciliation will happen.
* How the final insights are going to be stored and served, with a preferred type of database like time series DB.

**6. Design ecommerce estimated delivery time**

* Given an e-commerce website, we need to show the expected delivery time. There are going to be two types of expected delivery time. One which will be shown on the product page, which can have a higher error margin, and the one that will be shown post-checkout, which needs to be more accurate.
* For the first case, we need low latency and high throughput, and hence the expected delivery time needs to be precomputed. You need to finalize the pivots with the interviewer, which are going to be used for estimation. This can be something like (seller pincode, buyer pincode).
* Design the complete data ingestion pipeline, forming curated features, running rules and ML Models on top of it, generating insights and serving pipeline for this.

**7. Design a URL Shortener with Multi-tenancy**

* This service will generate short names for long URL’s and will redirect these short names to original URL’s when accessed.
* The service should be highly available and read request should have very low latency and very high throughput.
* The creation of short names should be transactional. The short name mappings should have TTL concept and there shouldn’t be any clash between the short names.
* How the system is going to work in multi tenant environment. How capacity and storage planning is going to work in such scenario.
