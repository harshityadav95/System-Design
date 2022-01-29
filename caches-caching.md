# Caches / Caching



**Types Of Caches:**

1. Application Server Cache
2. Distributed Cache
3. Global Cache
4. CDN

**Application Server Cache:**

Placing cache on the request node itself creates local storage. Whenever a request comes to the request node it returns the cached response from its local cache when there is no cached response it queries the disk and cache the response.

The problem is when there are many instances of the same request node in a distributed environment a load balancer is used to distribute requests to these nodes.

Since every request node has its own local storage for a cache when the request comes to the node which has not cached the response for the request that has been processed by other node causes cache miss. Then that request node has to query the disk and store the response it’s local. which causes repeated computation for the same request when request is distributed to another instance of the request node.

The solution to this problem is to maintain a distributed/Global cache

**Distributed Cache:**

In the distributed cache, the cache is distributed across multiple nodes

The cache is distributed using consistent Hashing, incase of consistent hashing the Hash function is independent of a number of cache nodes/objects. it is distributed on an abstract circle, to understand more about consistent hashing please read from [https://en.wikipedia.org/wiki/Consistent\_hashing](https://en.wikipedia.org/wiki/Consistent\_hashing)

So when the request comes request node knows where to look for the cached data using consistent hashing.

we can accommodate more cache nodes by adding nodes to the cache pool.

**Global Cache:**

In the case of the global cache, only one cache is maintained for all the request nodes.

When there is a cache miss there are two ways data is retrieved

. Global Cache queries the disk and caches the data

. Global Cache calls the request node and then caches the response from it**.**

**CDN(Content Distributed Network):**

CDN is a kind of cache that comes into play When you have a large amount of static media.

When there is a cache miss in CDN it will start request backend servers, then cache the data and serve the requesting user

When the system we are building isn’t large enough for CDN we can make the future transition easily by hosting it in a simple HTTP server like Nginx and then host the DNS in CDN instead of your local servers.

C**ache Invalidation:**

Cache invalidation is used to maintain the data coherent with the source of data(i.e Database), when there is a write operation recently the data needs to be consistent in cache and database.

Types of Cache Invalidation:

1. **Write Through Cache:**

In this scheme, data is written into the cache and corresponding database at the same time. so nothing gets lost in case of a system crash, power failure, or any other system disruptions

but the disadvantage is when you have lots of write operations, updating both in cache and database can cause latency issues.

**2. Write Around Cache:**

In this scheme, data is written into the permanent storage directly bypassing cache, this can help to overcome cache being flooded with write operations.

here the disadvantage is the most recent write can cause cache miss, so the request node needs to query the permanent storage and cache the response.

**3. Write-back Cache:**

In this scheme, the write operation is done into the cache alone, completion is confirmed to the client immediately, after certain intervals of time it is updated in the permanent storage.

This results in high throughput and low latency for write-intensive applications

but the problem is in case of a system crash, power failure or any other adverse event that causes data loss.

C**ache Eviction:**

When the cache is full it needs to be evicted to accommodate space for new data.

There are many cache eviction policies

1. **FIFO(First In First Out):**

Cache block which has been accessed first would be evicted first. Without any regard to how many times they were accessed in the past

**2 .LIFO(Last In Last Out):**

Cache block which has been accessed last would be evicted first. Without any regard to how many times they were accessed in the past

**3.LRU(Least Recently Used):**

Cached data that has not been accessed for a longer period of time is chosen to evict.

**3.MRU(Most Recently Used):**

Cached data has been accessed most recently is chosen to evict.

**4.LFU(Least Frequently Used):**

Maintains the count of how frequently the cached data is accessed, among them least is used for eviction.

**5.Random Replacement:**

Randomly selects a candidate item and is discarded to make space when necessary.



### Reference &#x20;

* &#x20;[Everything you need to know about Caching — System Design](https://levelup.gitconnected.com/everything-you-need-to-know-about-caching-system-design-932a6bdf3334)
* [Distributed cache system design](https://medium.com/system-design-concepts/distributed-cache-system-design-9560f7dd07f2)
