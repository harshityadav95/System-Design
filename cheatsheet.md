# CheatSheet

## System Design Cheat Sheet <a href="#4a52" id="4a52"></a>

[![Vivek Singh](https://miro.medium.com/fit/c/28/28/1\*jkV2ByMnOZJiNTHJiXSMfA.jpeg)](https://vivek-singh.medium.com/?source=post\_page-----318ba2e34723--------------------------------)[Vivek Singh](https://vivek-singh.medium.com/?source=post\_page-----318ba2e34723--------------------------------)[May 22·10 min read](https://vivek-singh.medium.com/system-design-cheat-sheet-318ba2e34723?source=post\_page-----318ba2e34723--------------------------------)

Reference: [Tech Dummies](https://www.youtube.com/channel/UCn1XnDWhsLS5URXTi5wtFTA) , [System Design](https://www.youtube.com/channel/UC9vLsnF6QPYuH51njmIooCQ), [Netflix](https://www.youtube.com/watch?v=psQzyFfsUGU), [GeeksForGeeks](https://www.geeksforgeeks.org/system-design-netflix-a-complete-architecture/)

For you to go through just before the interview :).

_**Disclaimer: This article is still under work. Still have to add notes and more architecture designs. Feel free to add comments on what else to add.**_

## Load Balancers (LB) <a href="#ee52" id="ee52"></a>

> Selects Servers/Databases/Caches following [some algorithm](https://kemptechnologies.com/load-balancer/load-balancing-algorithms-techniques/)

1. Round Robin: Select servers one after another
2. Weighted Round Robin: Admin assigns weight, i.e. probability…
3. Least Connection/Response Time/ Resource Based: Dynamic Load balancing. Server with Least Connection/Response Time/ Resource Based is allotted next request. The values are calculated using client installed at servers.
4. Similarly we have Weighted flavor of Least Connection/Response Time/ Resource Based.

[Types](https://freeloadbalancer.com/load-balancing-layer-4-and-layer-7/): (Ex [AWS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-types.html#clb))

* L4 : Makes balancing decision only on IP address , tcp port. Cannot see request header, client, type etc.
* L7 : Has info about url, message, request type, header, client everything. Can route request based on type of request

> Use L4 when you need to make simple reliable and fast balancing decision on server load, **which has reliable TCP connection.** Use L7 when you need to route request to appropriate resource server, such as image request will go to image server etc.

_**Types of Load Balancers in AWS : ELB, ALB, NLB**_

See Table : [https://iamondemand.com/blog/elb-vs-alb-vs-nlb-choosing-the-best-aws-load-balancer-for-your-needs/](https://iamondemand.com/blog/elb-vs-alb-vs-nlb-choosing-the-best-aws-load-balancer-for-your-needs/)

## Caches <a href="#4606" id="4606"></a>

> [High Speed Key Value storage.](https://www.imaginarycloud.com/blog/redis-vs-memcached/) Two popular Implementation Memcached (used by Netflix & Facebook) & Redis (used at [Pinterest ](https://tanzu.vmware.com/content/blog/using-redis-at-pinterest-for-billions-of-relationships))

_Use Memcached when_ : **Simple Key Value storage needed**. Need to only store string. No need to perform any operational query on cache. Scaling vertically by using more cores and threads is easier. When keys are maximum 250 B and Values maximum 1MB, when you are ok with only LRU eviction policy. **Its Volatile.**

_Use Redis when:_ You need to store objects (**don't want to serialize deserialize : access or change parts of a data object without having to load the entire object**). Scaling horizontally is easier. You need to store Set, Hash, List, Sorted Set (A non-repeating list of string values ordered by a score value). When you want to chose from multiple eviction policies**. When you would want to save data (its non volatile)**

[Eviction policies](https://www.imaginarycloud.com/blog/redis-vs-memcached/)

* **No eviction** returning an error the memory limit is reached.
* **All keys LRU** removing keys by the least recently used first
* **Volatile LRU** removing keys, that have an expiration time set, by the least recently used first.
* **All keys random** removing keys randomly
* **Volatile random** removing keys, that have an expiration time set, randomly
* **Volatile TTL** removing keys, that have an expiration time set, by the shortest time to live first.

## Queues <a href="#8792" id="8792"></a>

Two popular async message queuing services:[ RabbitMQ and Kafka](https://betterprogramming.pub/rabbitmq-vs-kafka-1ef22a041793). Supports _message queuing_ and _publish/subscribe._

> Message Queue: Used to decouple producer from consumer. Name a queue as X, publish to X, consume from X. Can be used for events based intra service communication. Consumer listen for events in queue.

![](https://miro.medium.com/max/30/0\*avXMbn2b5t2olJdn.png?q=20)![](https://miro.medium.com/max/700/0\*avXMbn2b5t2olJdn.png)Message Queuing

> Publish/subscribe: Used in Notification system, backend job that takes lot of time and one action that triggers multiple services. Provides : lose coupling between message production and consumption, fault tolerance, retry messages failed to be consumed and independently scalable.
>
> Can be done in following ways

![](https://miro.medium.com/max/30/0\*hcojcRTgLJ3yWAgx.png?q=20)![](https://miro.medium.com/max/500/0\*hcojcRTgLJ3yWAgx.png)

_Message Exchange ( as in Rabbit MQ, SQS):_ Producers submit messages to exchange. Consumers create a queue, subscribes to exchange. Exchange sends message to appropriate queues. It also takes care of failure, retry, expiry, routing. **Ordering of consumption not guaranteed.**![](https://miro.medium.com/max/30/0\*nXtY0bETuj6ak7GI.png?q=20)![](https://miro.medium.com/max/500/0\*nXtY0bETuj6ak7GI.png)

_Streaming Platform ( as in Kafka, Kinesis):_ Producers just write to their specific logs(Commit log/Write ahead logs). Consumers read data from logs using offset. Failures, retries, deletion, filtering all handled by consumers. **Order is guaranteed per consumer per partition per topic.** Replaying past message is possible(move the offset). Uses Zookeeper ( store information about brokers, topics, partitions, partition leader/followers, consumer offsets, etc.).

Topics in Kafka is like table in database. Thus, each message is associated with topic. Consumers subscribe to topic ( like update event on table). Multiple subscribers read from a topic( eg, one can send mail, other send phone message on order ship message in shipping topic). Messages not deleted on reading ( can have TTL), and written on disk. Since its sequential write its faster.

Central system managing queue is called Broker.

_See Kafka Use cases_ : [https://kafka.apache.org/uses](https://kafka.apache.org/uses)

_See RabbitMQ usecases_ : [https://www.rabbitmq.com/getstarted.html](https://www.rabbitmq.com/getstarted.html)

1. Work Queues : Distributing tasks among workers (the [competing consumers pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/CompetingConsumers.html)) : Cron job.
2. Pub Sub: Sending messages to many consumers at once : Notification service, intra service communiation
3. Routing: Receiving messages selectively : Queuing, retries
4. Topics: Receiving messages based on a pattern (topics) : Selective Notification
5. RPC: [Request/reply pattern](http://www.enterpriseintegrationpatterns.com/patterns/messaging/RequestReply.html) example.

![](https://miro.medium.com/max/30/0\*TdheUujhVd3N7tb5.png?q=20)![](https://miro.medium.com/max/372/0\*TdheUujhVd3N7tb5.png)

[**SNS SQS**](https://docs.aws.amazon.com/sns/latest/dg/sns-common-scenarios.html)

In SNS SQS, SQS is simply a queue, SNS on the other hand behaves like a broker. SQS behaves as subscribers EC2 behaves as consumers of SQS messages So SNS+SQS = Kafka. SQS alone is not. SQS is more like like partition in Kafka.

## Configuration Service <a href="#e307" id="e307"></a>

[Zookeeper ](https://intellipaat.com/blog/what-is-apache-zookeeper/#:\~:text=Apache%20ZooKeeper%20is%20used%20for,uses%20ZooKeeper%20to%20manage%20configuration.): Provides configuration information, naming, synchronization and group services over large clusters in distributed systems.

> Used to manage cluster of servers, databases or caches. External services can interact with clusters via zookeeper. Zookeeper names services, knows their ips, elects leader, provides failure recovery.

## API Gateway vs Service Mesh <a href="#ab4f" id="ab4f"></a>

Popular apps: Zuul, [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html). Read : [My experiences with API gateway](https://mahesh-mahadevan.medium.com/?url=https%3A%2F%2Fmahesh-mahadevan.medium.com%2Fmy-experiences-with-api-gateways-8a93ad17c4c4)

A service mesh’s primary purpose is to manage internal service-to-service communication, while an API Gateway is primarily meant for external client-to-service communication. [Read more.](https://dzone.com/articles/api-gateway-vs-service-mesh)![](https://miro.medium.com/max/500/0\*xzcxWjWxHimEMXdL.jpg)[API gateway pattern (microservices.io)](https://microservices.io/patterns/apigateway.html)

API Gateway provides a single entry point for a client for a number of different underlying APIs (system interfaces/web services/Rest APIs, Lambdas, etc.). Performs traffic management, authorization and access control, monitoring, and API version management.

API Gateway operates at Layer 7. Just like layer 7 load balancers it knows about content. Thus induces an overhead in extra hop at API gateway.![](https://miro.medium.com/max/30/0\*tALjg1HN71\_UK4bg.png?q=20)![](https://miro.medium.com/max/700/0\*tALjg1HN71\_UK4bg.png)[API Gateway vs. Service Mesh — DZone Microservices](https://dzone.com/articles/api-gateway-vs-service-mesh)

[Service Mesh](https://www.nginx.com/blog/what-is-a-service-mesh/) (Popular example Istio) operates at layer 4. Provides inter service communication. Works like a sidecar to services. Handles service discovery, circuit breaking, timeouts, retries, encryption, tracebility, authentication and authorization among services.

## CDN <a href="#017e" id="017e"></a>

Content Delivery Networks (Akamai, Cloudfare etc) : Mostly store static data which can be pushed by servers or CDN can pull from servers on a miss or at speculated times. Globally distributed so stays near clients and is fast.

> Use when you have some static data to serve near client, like images of restaurants/food on yelp, telephone directory

## How to Scale database <a href="#430e" id="430e"></a>

Reference: [Understanding database scaling patterns | by Kousik Nath | Medium](https://kousiknath.medium.com/understanding-database-scaling-patterns-ac24e5223522)

Brief: Query optimization -> vertical scaling -> Master slave -> multi master -> partitioning -> sharding -> multi-datacenter replication

## Cassandra <a href="#58a5" id="58a5"></a>

1. Wide Column NoSQL
2. Tunable consistency -> read by qourum. type of quorum defines consistency level
3. Fast wright -> Writes in log sequentially. Tombstoning and Compation in backend.
4. [A KKV value storage](https://wiki.atlan.com/apache-cassandra/) \[Like HashMap(partition) of HashMap(Rows or clusters)]: First K is the partition key and is used to determine which node the data lives on and where it is found on disk. The partition contains _multiple rows_ within it . Second K (clustering key) finds the row within a partition.\
   The clustering key (Second K) acts as both a primary key within the partition and how the rows are sorted. You can think of a partition as an ordered dictionary.

See how [Discord](https://blog.discord.com/how-discord-stores-billions-of-messages-7fa6ec7ee4c7#.dzqq7q4o7) used it to scale their storage. They made KK in KKV as `((channel_id, bucket), message_id)`. Message id was a [snowflake id](https://blog.twitter.com/engineering/en\_us/a/2010/announcing-snowflake).

## Snowflake at Twitter <a href="#1d82" id="1d82"></a>

Generates tens of thousands of ids per second in a highly available manner and is 64 bits (UUID is 128)

These ids need to be _roughly sortable_, meaning that if tweets A and B are posted around the same time, they should have ids in close proximity to one another since this is how most Twitter clients sort tweets.

To generate the roughly-sorted 64 bit ids in an uncoordinated manner, ids are generated as composition of: **timestamp, worker number and sequence number.**

Sequence numbers are per-thread and worker numbers are chosen at startup via zookeeper.

## Some numbers <a href="#293b" id="293b"></a>

**Availability** : 99.99% \~ 50 min downtime/year | 99.999 % \~ 5min downtime /year | 99.9999% \~= 30seconds downtime/year

**Bandwidth** : Average EC2 instance is 5Gb/s

**Requests/second :** On average 1 server can process 1000 requests/second

**Max Websocket Connection**: 65k (65,536) socket connections i.e max number of TCP ports available. _Ea_[_ch server can handle 65,536 sockets per single IP address_](https://dzone.com/articles/load-balancing-of-websocket-connections)_. So the quantity can be easily extended by adding additional network interfaces to a server. (HaProxy, ELB does TCP balancing as well)_

Low end dedicated MySQL server (2 cores, 4 GB RAM) can serve 100 requests/sec on an average \~10 million/day with CPU idle rate of 90%.

## Some More Numbers <a href="#bfe5" id="bfe5"></a>

Approximation : 1 day \~ 10⁴ seconds

**Daily active users :** Twitter: \~200M/day (200/sec), Facebook: \~2Bn (2000/sec), Whatsapp: \~200M, Netflix: 200M/day

**Photos uploaded:** 200M/day (200/sec)(Instagram)

**Videos uploaded:** 500 hours/minute (Youtube)

**Uber:** 20 million trips/day (20\*10⁶ / day => 2000/sec)

**Bandwidth :** Say 200 reads/second, each read needs 10kb data (10⁴ characters ) 2000kb/sec => 2MB/sec

## ……………….Architecture Designs………………….. <a href="#7958" id="7958"></a>

## Instagram <a href="#522e" id="522e"></a>

Reference: [System Design Analysis of Instagram](https://towardsdatascience.com/system-design-analysis-of-instagram-51cd25093971)![](https://miro.medium.com/max/1000/0\*p1uHjnc9-mMfDu3Z.jpeg)

* Precompute : User A posts “x”. User B,C,D follows. Then “x” can be appended to User B,C,D news feed inside cache. This is called fanning out. In hybrid approach, if A is a celebrity, then its post is not fanned out ( 1 post need to be written to millions cache).
* Notification server needs to maintain a persistent websocket connection to push data to User B
* Place Metadata service to : provide separation of concern, access to DB via API, also can act as a caching layer, metadata can be accessed via service cache rather than db.

## Uber <a href="#aada" id="aada"></a>

Reference: [UBER System design](https://www.youtube.com/watch?v=umWABit-wbk)

[Best Article so far](https://kousiknath.medium.com/system-design-design-a-geo-spatial-index-for-real-time-location-search-10968fe62b9c): A must read![](https://miro.medium.com/max/30/0\*\_ZViEzWdqPsgQpim.jpeg?q=20)![](https://miro.medium.com/max/700/0\*\_ZViEzWdqPsgQpim.jpeg)

## Web Crawler <a href="#b194" id="b194"></a>

Reference: [RoadtoArchitect](https://roadtoarchitect.com/2019/04/01/design-web-crawler/)![](https://miro.medium.com/max/700/0\*4lZwYRMf3CBKy6PH)[https://www.educative.io/collection/page/5668639101419520/5649050225344512/5718998062727168](https://www.educative.io/collection/page/5668639101419520/5649050225344512/5718998062727168)

Note:

Get document, remove duplicate documents using checkum, extract URLs in page, remove duplicate urls, feed to the queue to parse.

Fetcher will map host name to robots.txt values.

**Domain name resolution:** Before contacting a Web server, a Web crawler must use the Domain Name Service (DNS) to map the Web server’s hostname into an IP address. DNS name resolution will be a big bottleneck of our crawlers given the amount of URLs we will be working with. To avoid repeated requests, we can start caching DNS results by building our local DNS server.

## URL Shortner <a href="#a290" id="a290"></a>

Reference: [Educative](https://www.educative.io/courses/grokking-the-system-design-interview/m2ygV4E81AR)![](https://miro.medium.com/max/30/1\*hT52U-QpI9naqfkZGOqYIQ.png?q=20)![](https://miro.medium.com/max/700/1\*hT52U-QpI9naqfkZGOqYIQ.png)Detailed components![](https://miro.medium.com/max/30/1\*DIVthTIhsWrtzWfq\_NuUKw.png?q=20)![](https://miro.medium.com/max/700/1\*DIVthTIhsWrtzWfq\_NuUKw.png)

Key point here is a separate key generation service and cleanup service

## Yelp <a href="#d514" id="d514"></a>

Reference: [Tech Dummies](https://www.youtube.com/watch?v=TCP5iPy8xqo\&t=1687s)![](https://miro.medium.com/max/30/0\*DJlIq85nSCwg02Yo.png?q=20)

![](https://miro.medium.com/max/700/0\*DJlIq85nSCwg02Yo.png)

## Dropbox <a href="#9736" id="9736"></a>

Resource: [Dropbox system design](https://www.youtube.com/watch?v=U0xTu6E2CT8\&t=976s)![](https://miro.medium.com/max/700/0\*\_Boik-e6LBFAnP\_4.jpeg)

## Distributed Message Queue <a href="#8c04" id="8c04"></a>

Resource : [System Design Interview](https://www.youtube.com/watch?v=iJLL-KPqBpM)![](https://miro.medium.com/max/700/1\*CEiMO3pL2y2ZKA2\_vCnkSQ.png)

## Distributed Cache <a href="#922e" id="922e"></a>

![](https://miro.medium.com/max/700/1\*MTGvCmplegkGAfUdc2cEMA.png)Unsharded![](https://miro.medium.com/max/700/1\*fXD0MTZhZK0l7QkmXACz4Q.png)Sharded

## Distributed Cache at Netflix <a href="#3eb7" id="3eb7"></a>

Reference: [Netflix](https://www.youtube.com/watch?v=psQzyFfsUGU)![](https://miro.medium.com/max/1000/0\*w9jhKM7SS6vFNcEu)Netflix Implementation![](https://miro.medium.com/max/700/1\*XARlPM0Cy3BviZHphs8WDA.png)Level 1![](https://miro.medium.com/max/700/1\*392ZKwSB2cCODAnK6ehbLw.png)Level 2

## Twitter <a href="#fd08" id="fd08"></a>

Reference: [Tech Dummies](https://www.youtube.com/watch?v=wYk0xPP\_P\_8\&t=817s)![](https://miro.medium.com/max/1000/0\*oWJzwcZNmeWnqTPE.png)

## Netflix <a href="#453a" id="453a"></a>

![](https://miro.medium.com/max/1000/0\*t5beLkCoxuvxRZbe.png)

## Distributed Rate Limiter <a href="#7ece" id="7ece"></a>

Resources: [System design Interview](https://www.youtube.com/watch?v=FU4WlwfS3G0\&t=1378s)

* Client Identifier Builder assigns unique id/client to get originator.
* Rate limiter coordinates with throttler and processor to pass or reject.
* Throttling Service implements token bucket, sliding window using requests stored in rules db.

![](https://miro.medium.com/max/30/1\*GV66HQxeM30l5BS-J1dQYg.png?q=20)![](https://miro.medium.com/max/700/1\*GV66HQxeM30l5BS-J1dQYg.png)![](https://miro.medium.com/max/30/1\*0tI9pvkDHzDnROV2hLigFA.png?q=20)![](https://miro.medium.com/max/700/1\*0tI9pvkDHzDnROV2hLigFA.png)Token Bucket algorithm![](https://miro.medium.com/max/30/1\*BWilm4\_dT8DZ1Eix7o90yQ.png?q=20)![](https://miro.medium.com/max/700/1\*BWilm4\_dT8DZ1Eix7o90yQ.png)

## Notification Service <a href="#7bcf" id="7bcf"></a>

Resources: [System design Interview](https://www.youtube.com/watch?v=bBTPZ9NdSk8) . Kind of like building RabbitMQ![](https://miro.medium.com/max/700/1\*8XAHqkJQaNdh8mpHhbX25g.png)

* Metadata service for caching and separation of concern.
* Temporary storage to store message for a while
* Sender reads data from Metadata service to know which message to send to whom.

![](https://miro.medium.com/max/30/1\*NMjyGKnhAo069jE1aXK0XQ.png?q=20)![](https://miro.medium.com/max/700/1\*NMjyGKnhAo069jE1aXK0XQ.png)![](https://miro.medium.com/max/30/1\*C5E8DiMcYIBqMH-jAkovOQ.png?q=20)![](https://miro.medium.com/max/700/1\*C5E8DiMcYIBqMH-jAkovOQ.png)

Frontend Service Host caches the metadata![](https://miro.medium.com/max/30/1\*x0wczYhFSL1A6pxgRUR-tQ.png?q=20)![](https://miro.medium.com/max/700/1\*x0wczYhFSL1A6pxgRUR-tQ.png)

Temporary storage can be NoSQL, Key Value (Redis) or Column Based (Cassandra) or streaming service (kafka)

## Message sharing within cluster <a href="#2d06" id="2d06"></a>

Resources: [System design Interview](https://www.youtube.com/watch?v=FU4WlwfS3G0\&t=1378s)![](https://miro.medium.com/max/700/1\*eWswF-CpAGZIXcRJmOZB2Q.png)

420
