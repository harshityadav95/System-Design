# URL Shortner

## Introduction <a href="#e078" id="e078"></a>

System design Interview problems are intentionally open-ended. It gives an interviewer opportunity to evaluate design skills and also the depth of technical concepts. Day-to-day engineering work involves architecting a lot many scalable systems. This interview provides a platform to judge a candidate’s problem-solving & communication skills as well.

It’s difficult to nail a System Design round without sufficient preparation & practice. Also, it’s intimidating for candidates who don’t have a background in Distributed Systems. At times, interviewees seem to lose their focus & deviate from the topic if they don’t follow a structured methodology.

It’s essential to have conceptual clarity of system design concepts before tackling a problem. In this article, I have chosen a simple problem of designing a URL Shortening service. It’s an easy problem for any beginner. There is no concrete right or wrong answer to a system design problem. However, the candidate has to justify the design choices made while solving the problem.

We will be looking at how we must go about solving a design problem systematically. Along the way, I’ll also touch upon distributed system fundamentals, APIs, Database Schema design & design trade-offs. Let’s get started.

![](https://miro.medium.com/freeze/max/60/1\*5hZ\_iPBOkgchfeATXXTS3Q.gif?q=20)![](https://miro.medium.com/max/792/1\*5hZ\_iPBOkgchfeATXXTS3Q.gif)**Let’s start the design**

## Understanding the problem/product <a href="#11e4" id="11e4"></a>

The problem statement here is ‘_Design a URL Shortner like tinyurl.com_’. If you don’t know or have never heard of [TinyUrl.com](https://tinyurl.com), take a pause & visit the website. Before beginning the discussion, one needs to understand the product & it’s behaviour.

![](https://miro.medium.com/max/60/1\*0hy5GKdpHwZ9Aq7sBRor7w.png?q=20)![](https://miro.medium.com/max/1540/1\*0hy5GKdpHwZ9Aq7sBRor7w.png)**Tiny URL**

Having used the product will give you an idea of how clients interact with it. Further, it’s always good to ask the interviewer a few clarifying questions —

* Who are the users of this system — _Enterprise users or in general any user_
* How are they going to use it — _Web Browsers or Mobile devices_
* Application’s goal — _Shorten any URL on the internet or organization’s internal URL_
* Can customers provide custom short URLs?
* Data storage — Temporary short URLs or permanent ones

The responses to the above questions will help one gain clarity about the problem. Asking questions also shows that an individual doesn’t jump into the solution. It helps one to clear any doubt and proceed with the design with enough clarity.

## Requirements <a href="#c5b8" id="c5b8"></a>

Let’s break our requirements into Functional & Non-Functional requirements.

Following will be the Functional requirements:-

1. The system should convert a long URL to a short URL
2. On typing short URL in the browser, the user must be redirected to the long URL link
3. User should have the ability to provide a custom short URL for a given long URL
4. The system should store the URL for a year & later purge all old entries

Following will be the Non-Functional requirements:-

1. The service must be highly available & scalable
2. It should handle high throughput with minimum latency
3. The application should be durable and fault-tolerant

As we have clarity on what needs to be designed, we can proceed to the next step in design.

## CP vs AP System <a href="#3859" id="3859"></a>

We need to decide whether the system will be a CP or an AP system.

For URL shortener, are we fine to compromise Consistency over Availability? This is something that needs discussion with the Interviewer.

In this case, let’s assume that if a new URL is created it may not be immediately available for use. For eg:- A newly created URL can be available after a delay of few milliseconds. Until then, the system will throw an error indicating the URL is not found. The system will be highly available and continue to respond to the user’s queries.

As we have chosen to design an AP system, every read request may not follow the most recent write. However, the system will be eventually consistent.

## Capacity Estimation <a href="#8648" id="8648"></a>

### Traffic Computation <a href="#51fa" id="51fa"></a>

While designing a large-scale system, we need to know how many users will use the system. What will be the data access patterns? how many application & data servers will be needed.

The answer to the above questions in guestimation. The candidate is supposed to come up with a rough estimate of how many users will use the service?

Assume that we have 200 Mn users. Out of the 200Mn users, 2 Mn users can be actively shortening the URL. For each day, on average, let’s say 10 requests (read + write) are received.

So, we have 20 Mn such records created every day. The remaining 20Mn users would be getting the long URL back from the short URL. Assuming each user fires 10 requests, we have an overall 200 Mn request. This turns out to be 2300 requests/per sec.

![](https://miro.medium.com/freeze/max/60/1\*64mqpJ2miTpB1bdD2mWn2w.gif?q=20)![](https://miro.medium.com/max/770/1\*64mqpJ2miTpB1bdD2mWn2w.gif)**Traffic?**

### Storage Computation <a href="#8631" id="8631"></a>

20 Mn URLs are generated every day. Let’s say the system is designed to handle requests for the next five years.

Total URLs to be stored = 20 Mn \* 30 (days) \* 12 \* 5 = _36 Bn_

We will store long URL, short URL, created and modified fields in the data store. Every record will consume _0.5 Kb_ of memory. Thus, the total storage needed will be **36 Bn \* 0.5 Kb** = _18 TB_

## APIs <a href="#43a4" id="43a4"></a>

As agreed in the requirements, we will have the below APIs:-

* **Shorten long URL**

![](https://miro.medium.com/max/60/1\*vAKKBMjjoRXUfOKMULTGDQ.png?q=20)![](https://miro.medium.com/max/1540/1\*vAKKBMjjoRXUfOKMULTGDQ.png)**Shorten long URL**

The request body will take _customShortUrl_ as an optional parameter. If not present, it will assume empty.

Backend server will perform validation on the length of the long URL. Accordingly, it will return the following response codes:-

▹ 400- Bad request

▹ 201- Created. Url mapping entry added

▹ 200- OK. Idempotent response for the duplicate request

* **Fetch long Url from short URL**

![](https://miro.medium.com/max/60/1\*aJ7kyPnSxEXvj\_nnLzT4mQ.png?q=20)![](https://miro.medium.com/max/1540/1\*aJ7kyPnSxEXvj\_nnLzT4mQ.png)**Long URL from short URL**

If the system finds the long URL corresponding to the short URL, the user will be redirected. The HTTP response code will be 302.

The service will throw a 404 (Not Found) in case the short URL is absent. It will perform a length check on the input. In case, the length exceeds the threshold, 400 (Bad Request) will be thrown.

## Building Blocks & URL Shortening Algorithm <a href="#8c70" id="8c70"></a>

### Key Components <a href="#3269" id="3269"></a>

Following will be the key components in the URL shortening application:-

1. Clients- Web Browsers/Mobile app. It will communicate with the backend servers via HTTP protocol
2. Load Balancer- To distribute the load evenly among the backend servers
3. Web Servers- Multiple instances of web servers will be deployed for horizontal scaling
4. Database- It will be used to store the mapping of long URLs to short URLs

Below is a high-level sketch of the components and interaction between them:-

![](https://miro.medium.com/max/60/1\*JX1HgL9Aaoedz5yWR81dXA.png?q=20)![](https://miro.medium.com/max/1540/1\*JX1HgL9Aaoedz5yWR81dXA.png)**High-level Design of the system**

### Shortening Algorithm <a href="#d6ec" id="d6ec"></a>

We estimated that the system would need to store 36 Bn URLs. The short URL can consist of lower-case (_‘a’ — ‘z’_), upper-case (_‘A’-’Z’_) & numbers (_0–9_). To support 36 Bn URLS, the short URL must be at least 6 characters long.

⁶²⁶ = 56.8 Bn.

One of the simplest approach to convert a long URL to a short URL is to use hashing. Pass the long URL to a hashing function to obtain a fixed-length string (For eg:- 32-byte string). Then extract the first 6 characters to get the short URL.

However, there are two downsides of using the above approach:-

1. Two different URLs can map to the same short URL. This is as a result of collisions. Using a uniform hash function can reduce the chances of a collision. Additionally, a pseudo-random number can be used for hashing
2. If a User tries to shorten the same long URL multiple times, he should get a different result every time. In this case, due to collision, chances that the long URL returns the same short URL are high

### URL Generator service <a href="#9ec3" id="9ec3"></a>

We need to guarantee uniqueness for every long URL that is being shortened. For this, we can pre-generate a set of short URLs and persist it in the database. A new layer can be introduced to manage these short URLs. Let’s call this service layer _URL Generator Service._

The service will act as a source of all short URLs. The URL Shortener service will get a new short URL from this service. Subsequently, it will store the mapping between long & short URLs. Following diagram illustrates this process:-

![](https://miro.medium.com/max/60/1\*zsYsgyE-76KyORkaG1g9zg.png?q=20)![](https://miro.medium.com/max/1540/1\*zsYsgyE-76KyORkaG1g9zg.png)**Role of URL Generator Service**

The above process will ensure that every request is allocated a unique short URL. There are a few challenges with the above approach. We considered the case of a single service requesting a tiny URL. What if multiple URL Shortener services running are running?

### Concurrency Issues <a href="#7eba" id="7eba"></a>

Scaling the above system may result in concurrency issues. If not handled correctly, the same URL may get allocated to two different services.

![](https://miro.medium.com/max/60/1\*hghLsPfdIRZp4cD8j4tOug.png?q=20)![](https://miro.medium.com/max/1540/1\*hghLsPfdIRZp4cD8j4tOug.png)**Scaling the URL Shortener service**

To solve this, we can associate state to a tiny URL. The tiny URL can have two states — _ACTIVE_, and _INACTIVE_. By default, all URLs will be in an _INACTIVE_ state. Once it’s allocated to a client, it must be marked as _ACTIVE_. Only _ACTIVE_ URLs can be assigned to clients.

To improve latency, the _URL Generator Service_ can prefetch all tiny URLs from the datastore. This will avoid database calls for every new request. Moreover, it can now use a locking data structure to synchronize access by multiple client applications.

## Database Schema <a href="#5976" id="5976"></a>

We will need a single table to store the mapping between long URLs and short URLs. Following schema can be used:-

**URL Mapping schema**

Since the _short\_url_ column is a primary key, its indexed and lookups will be quick. Two timestamp fields _created\_at_ and _modified\_at_ have been included. These fields will help to purge out all the old entries in the table.

To decide between RDBMS or No-SQL database system, let’s revisit our storage estimates. We want to store at least 18 TB of data. Further, we want to serve the requests with minimum latency.

It’s better to have the data distributed on different machines. This will prevent a single point of failure. Hence, data sharding will be necessary in this case.

Further, we don’t have a data model with relations. There is no requirement for joins here. Also, we expect our data to grow in size gradually. Additionally, our system is supposed to function as an AP system.

Taking the above points into account, NoSQL seems like a clear winner here. NoSQL databases have built-in sharding and are easier to scale.

## Trade-offs & Bottlenecks <a href="#251e" id="251e"></a>

### How to scale reads <a href="#6bbb" id="6bbb"></a>

* **Caching**- By introducing a cache, we can scale the read queries. If multiple queries are received with the same short URL, the service can return long URL from the cache. We can use LRU (Least Recently Used) as the Cache eviction policy.

![](https://miro.medium.com/max/60/1\*c1H91\_3Kks\_IS7QTVnWfVQ.png?q=20)![](https://miro.medium.com/max/1540/1\*c1H91\_3Kks\_IS7QTVnWfVQ.png)**Caching**

* **Data replication-** We can segregate the reads from the write queries. Writes can be performed on the master and data can be replicated to slaves. Slaves can be used for executing read queries.

![](https://miro.medium.com/max/60/1\*xiENAF-Hv0gEeakldBJsFw.png?q=20)![](https://miro.medium.com/max/1540/1\*xiENAF-Hv0gEeakldBJsFw.png)**Data replication**

### How to shard the data <a href="#70b4" id="70b4"></a>

Storing data on a single database server can cause bottlenecks. There can be a single point of failure. With growing traffic, the read/write pressure on a database will also increase. Hence, it’s better to distribute data on different database servers.

We have the following three strategies for sharding the data:-

1. **Range-based partitioning**- In this strategy, URLs starting with ‘_a-h_’ can be assigned to server 1, the ones starting with ‘_i-o_’ to server 2 and so on. With this approach, there is a possibility of uneven data distribution. This might result in one shard having twice or thrice the data of another
2. **Hash-based partitioning**- This method eliminates the possibility of uneven data distribution. However, it doesn’t scale well when new servers are introduced or existing ones are removed.
3. **Consistent Hashing**- This partitioning strategy overcomes the limitations of the above two strategies. The data distribution is even and the addition or removal of new servers has an impact on a minimum number of records.

### Cleanup of URLs <a href="#b023" id="b023"></a>

An asynchronous job needs to be scheduled to remove all the expired shortened URLs. This job will filter all the expired URLs and return them to the data store for future use. Once the URLs are cleaned up, the tiny URL’s state will be changed to _INACTIVE_. The service will continue to function as it is and allocate one of the _INACTIVE_ URLs to a new client request.

## System Design of URL Shortening Service <a href="#5922" id="5922"></a>

### System design is one of the _**most important and feared**_ aspects of software engineering. This opinion comes from my own learning experience in an associate architecture course. When I started my associate architecture course, I had a hard time understanding the idea of designing a system. <a href="#caa5" id="caa5"></a>

One of the main reasons was that the terms used in software architecture books are pretty hard to understand at first, and there is no clear step by step guidelines. Everybody seems to have a different approach.

So, I set out to [design a system](https://towardsdatascience.com/system-design-101-b8f15162ef7c?source=friends\_link\&sk=be3d26de1f9d1671b4abe379aae814f8) based on my experience of learning architecture courses. This is part of a series on system design for beginners (link is given below). For this one, let’s design the URL shortening service.

In [Medium](https://medium.com/u/504c7870fdb6?source=post\_page-----b325b18c8f88--------------------------------), we can see the URLs are pretty big, especially the friend links; while sharing an article, we tend to shorten the URL. Some of the known URL shortening services are TinyURL, bit.ly, goo.gl, rb.gy, etc. We are going to design such a URL shortening service.

### ★ Definition of the System: <a href="#5e6d" id="5e6d"></a>

We need to clarify the goal of the system. _System design is such a vast topic; if we don’t narrow it down to a specific purpose, then it will become complicated to design the system, especially for newbies. URL shortening service provides shorter aliases for long URLs. When users hit the shortened links, they will be redirected to the original URL._![#URLShorteningservice #tinyURLDesign #system design](https://miro.medium.com/max/9248/1\*S-Ic3p3vbUGS\_8bh2-wbmg.jpeg)Image by Author

### ★ Requirements of the System: <a href="#f9e9" id="f9e9"></a>

In this segment, we decide the features of the system. What are the requirements we need to focus on? **We may divide the system requirements into two parts:**

* **Functional requirement:**

The user gives a URL as an input; our service should generate a shorter and unique alias of that URL. When users hit the shorter link, our system should redirect them to the original link. Links may expire after a duration. Users can specify the expiration time. _We are not considering custom links by the user here._

This is a requirement that the system has to deliver. It is the main goal of the system.

*

Now for the more critical requirements that need to be analyzed. If we don’t fulfill this requirement, it might be harmful to the business plan of the project. So, let’s define our NFRs:

> The system should be highly available. If the service is down, all the URL redirections will fail. URL redirection should happen in real-time. Nobody should be able to predict the shortened links.

**Performance, modifiability, availability, scalability, reliability, etc. are some important quality requirements in system design. These ‘ilities’ are what we need to analyze for a system and determine if our system is designed properly.**

In this system, availability is the main quality attribute. Security is another important attribute. Normally, availability and scalability are important features for system design. Performance is by default important, nobody wants to build a system with worse performance, right?!

### ★ How much request does the system need to handle? <a href="#3d12" id="3d12"></a>

Let’s assume, one user may request for a new URL and use it 100 times for redirection. So, the ratio between write and read would be 1:100. So the system is read-heavy.

How many URL requests do we need to handle in the service? Let’s say we may get 200 URL requests per second. So, for a month’s calculation, we can have 30 days \* 24 hours \* 3600 seconds\*200 =\~ 500 M requests.

So, there can be almost 500M new URL shortening requests per month. Then, the redirection request would be 500M\*100 = 50 Billion.

For year count you have to multiply this number by 12.

### ★ How much storage do we need? <a href="#7f90" id="7f90"></a>

Let’s assume, the system stores all the URL shortening request and their shortened link for 5 years. As we expect to have 500M new URLs every month, the total number of objects we expect to store will be 500 M \* (5 \* 12) months = 30 B.

Now let’s assume that each stored object will be approximately 100 bytes. We will need total storage of 30 billion \* 100 bytes = 3 TB.

If we want to cache some of the popular URLs that are frequently accessed and if we follow the 80–20 rule, meaning we keep a 20% request from the cache.

Since we have 20K requests/second, we will be getting

_**20K \* 60 seconds\* 60 minutes \* 24 hours = \~1.7 billion per day**_

If we plan to cache 20% of these requests, we will need

_**0.2 \* 1.7 billion \* 100 bytes = \~34GB of memory.**_

### ★ Data flow: <a href="#6ef1" id="6ef1"></a>

_**For newbies to system design, please remember, “If you are confused about where to start for the system design, try to start with the data flow.”**_

Now, one of the main tasks of the server-side components is generating a unique key for an input URL. Here, our input data is only a URL. So, we need to store them as a string. The output is another shortened version of the URL. If somebody clicks on that shortened URL, it will redirect to the original URL. Now, each output URL needs to be unique.

### ★ Generate a short — unique key for a given URL <a href="#089b" id="089b"></a>

_For example, we may take a random shortened URL “_[rb.gy/ln9zeb](https://rb.gy/ln9zeb)_”. The last characters should form a unique key. So, our input is a long URL given by users._

We need to compute a unique hash of the input URL. If we use base64 encoding, 6 characters long key will give us 64 ^(6)= \~68.7 billion possible strings, which should be enough for our system.

**Problem:** If multiple users enter the same URL, the system should not provide the same shortened URL. What if some strings are duplicated, what would be the system’s behavior?

**Solution:** We may append the input URL with an increasing sequence number to each request URL. It should make the URL unique. But, the overflow of sequence numbers might be a problem. We may append user-id to the input URL assuming user-id be unique.

### ★ Unique Key Generation: <a href="#bf6c" id="bf6c"></a>

In the system, user-id should be unique so that we can compute a unique hash. We can have a standalone Unique-key Generation Service(UGS) that generates random id beforehand and stores them in a database.![#system design of tinyURL or URLShortening service](https://miro.medium.com/max/8404/1\*8ihEEDoWj-NdEON-PTAGKQ.jpeg)Figure: UGS service for unique key generation (Image by Author)

Whenever we need a new key, we can take one of the already generated IDs. This approach can make things faster as while a new request comes, we don’t need to create an ID, ensure its uniqueness, etc. UGS will ensure all the IDs are unique, and they can be stored in a database so that the IDs don’t need to be generated every time.

As we need one byte to store one character, we can store all these keys in:

6 (characters) \* 68.7B (unique keys) \~= 412 GB.

### ★Availability & Reliability: <a href="#0f4a" id="0f4a"></a>

If we keep one copy of UGS, it’s a single point of failure. So, we need to make a replica of UGS. If the primary server dies, the secondary one can handle the requests of the users.

**Each UGS server can cache some keys from key-DB.** It can speed things up. But, we have to be careful; if one server dies before consuming all the keys, we will lose those keys. But, we may assume, this is acceptable since we have almost 68B unique six-letter keys.

For ensuring availability, we need to ensure to remove a single point of failure in the system. Replication for Data will remove a single point of failure and provide backup. We can keep multiple replications to ensure database server reliability. And also, for uninterrupted service, other servers also need copies.

### ★DataStorage: <a href="#2a3c" id="2a3c"></a>

In this system, we need to store billions of records. Each object we keep is possibly less than 1 KB. One URL data is not related to another. So, we can use a NoSQL database like Cassandra, DynamoDB, etc. A NoSQL choice would be easier to scale, which is one of our requirements.

### ★ Scalability: <a href="#4116" id="4116"></a>

For supporting billions of URLs, we need to partition our database to divide and store our data into different DB servers.

i) We can partition the database based on the first letter of the hash key. We can put keys starting with ‘A’ in one server, ‘B’ in another server. This is called **Range Based Partitioning.**

The problem with this approach is that it can lead to unbalanced partitioning. For example, there are very few words starting with ‘Z.’ On the other hand; we may have too many URLs that begin with the letter ‘E.’

We may combine less frequently occurring letters into one database partition.

ii) We can also partition based on the hash of the objects we are storing. We may take the hash of the key to determine the partition in which we can store the data object. The hash function will generate a server number, and we will store the key in that server. This process can make the distribution more random. This is **Hash-Based Partitioning.**

If this approach still leads to overloaded partitions, we need to use [Consistent Hashing](https://en.wikipedia.org/wiki/Consistent\_hashing).

### ★ Cache: <a href="#279d" id="279d"></a>

We can cache URLs that are frequently accessed by the users. The UGS servers, before making a query to the database, may check if the cache has the desired URL. Then it does not need to make the query again.

What will happen when the cache is full? We may replace an older not used link with a newer or popular URL. We may choose the Least Recently Used (LRU) cache eviction policy for our system. In this policy, we remove the least recently used URL first.

### ★ Load balancer: <a href="#1815" id="1815"></a>

We can add a load balancing layer at different places in our system, in front of the URL shortening server, database, and cache servers.

We may use a simple Round Robin approach for request distribution. In this approach, LB distributes incoming requests equally among backend servers. This approach of LB is simple to implement. If a server is dead, LB will stop sending any traffic to it.

Problem: If a server is overloaded, the LB will not stop sending a new request to that server in this approach. We might need an intelligent LB later.

### ★ Link expiration after a duration: <a href="#884a" id="884a"></a>

If the expiration time is reached for a URL, what would happen to the link?

We can search in our datastores and remove them. The problem here is that if we chose to search for expired links to remove them from our data store, it would put a lot of pressure on our database.

We can do it another way. We can slowly remove expired links periodically. Even if some dead links live longer, it should never be returned to users.

If a user tries to access an expired link, we can remove the link and return an error to the user. A periodical clean up process can run to remove expired links from our database. As storage is getting cheaper, some links might stay there even if we miss while clean up.

After removing the link, we can put it back in our database for reuse.

### ★ Security: <a href="#be11" id="be11"></a>

We can store the access type (public/private) with each URL in the database. If a user tries to access a URL, which he does not have permission, the system can send an error (HTTP 401) back.![Final system design of TinyURL #URL Shortening service](https://miro.medium.com/max/11584/1\*Amr7wQ53nzcex\_poys4bPQ.jpeg)Figure: Final design of URL Shortening Service (Image by Author)

### Conclusion: <a href="#4cbf" id="4cbf"></a>

In this system, we did not consider the UI part. And as this is a web service, so no client part is also discussed either. The unique key generation is an important part of this system. So, we added an extra service to create and store unique keys for URLs. For ensuring the availability of services, we used replication of servers so that if one goes down, others can still give service. Databases are also replicated to ensure data reliability. The cache server is used to store some popular queries to speed up the latency. And load balancer is added to distribute incoming requests equally among backend servers.

Source: Grokking the System Design Interview Course.
