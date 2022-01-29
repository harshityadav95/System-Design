# LRU Cache

## How to Build an LRU Cache in Less Than 10 Minutes and 100 Lines of Code <a href="#2f86" id="2f86"></a>

### Master this common interview topic <a href="#09df" id="09df"></a>

[![Sun-Li Beatteay](https://miro.medium.com/fit/c/96/96/1\*F\_eq5XQVAZSAhXLv8xs-Mg.jpeg)](https://medium.com/@SunnyB?source=post\_page-----fddad56d7af5--------------------------------)[Sun-Li Beatteay](https://medium.com/@SunnyB?source=post\_page-----fddad56d7af5--------------------------------)[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F\_%2Fsubscribe%2Fuser%2F504ae0305598\&operation=register\&redirect=https%3A%2F%2Fbetterprogramming.pub%2Fhow-to-build-an-lru-cache-in-less-than-10-minutes-and-100-lines-of-code-fddad56d7af5\&user=Sun-Li%20Beatteay\&userId=504ae0305598\&source=post\_page-504ae0305598----fddad56d7af5---------------------follow\_byline-----------)[Jan 4](https://betterprogramming.pub/how-to-build-an-lru-cache-in-less-than-10-minutes-and-100-lines-of-code-fddad56d7af5?source=post\_page-----fddad56d7af5--------------------------------) · 8 min read![diagram of the structure of an LRU cache](https://miro.medium.com/max/2560/1\*HE8j1-iCHIZiiO9YOVYpAA.png)LRU cache (Image Credit: [https://corvostore.github.io/#LRU](https://corvostore.github.io/#LRU))

_Note: All the code in this article can be found on_ [_GitHub_](https://github.com/sunny-b/lrucache\_examples)_._

When I was interviewing for my first software engineering job, I was asked about least recently used (LRU) caches a number of times. I was asked to both code them and to describe how they’re used in larger systems. And if you’ve done your fair share of coding interviews, it’s likely you have, too.

From an interviewer's perspective, caching makes for a versatile topic. It can be used to gauge someone’s low-level understanding of data structures and algorithms. It can also be turned around to challenge one’s high-level comprehension of distributed systems.

For job candidates, however, it can be a jarring experience — especially if you’ve never used them in a professional setting. That was the case for me. The first time I was asked about LRU caches, my mind went blank. I’d heard of them, sure. But in all my studying of binary trees and heaps, I hadn’t bothered to learn what goes into them. And live in front of an interviewer isn’t the ideal setting to try to work it out.

While I didn’t do too well in that interview, that doesn’t have to be your fate. In this article, I’m going to teach you the what and how of LRU caches. By the end, you will know how to implement your own cache in less than 100 lines of code without any third-party libraries — all within ten minutes.

So let’s get going — time is ticking.

## What Is a Least Recently Used (LRU) Cache? <a href="#9410" id="9410"></a>

Caches are a type of data storage device that typically stores data in memory for fast retrieval. They are generally implemented as a key-value store, meaning you store and access the data via an identifying key of some sort.

The RAM on your computer is an example of a cache. The operating system stores data in RAM for faster access than the hard drive, and it uses the address of the memory cell as the key.![diagram of the structure of an operating system cache](https://miro.medium.com/max/1046/1\*vMjQS4WCtP5bS93c21X3sA.jpeg)Operating system caches (Image Credit: [https://www.sciencedirect.com](https://www.sciencedirect.com/topics/computer-science/disks-and-data))

LRU caches are a specific type of cache with a unique feature. When an LRU cache runs out of space and needs to remove data, it will evict the key-value pair that was least recently fetched from the cache.

Popular open source examples of LRU caches are [Redis](https://en.wikipedia.org/wiki/Redis) and [Memcached](https://en.wikipedia.org/wiki/Memcached).

## Why Are LRU Caches Useful? <a href="#05de" id="05de"></a>

For tech businesses that provide an API or user interface, performance and availability are crucial. Think about the last time you visited a slow website. Did you stay and wait for it to load, or did you leave and visit another site? Most likely the latter. Slow or under-performing websites can result in millions in lost revenue.

Unfortunately, many of the same systems that rely on high uptime also have to store mountains of data. This is often the case for social media and e-commerce sites. These sites store their data in a database of some sort, be it SQL or NoSQL. While this is standard practice, the problem comes when you need to fetch that data. Querying databases, especially when they contain a lot of data, can be quite slow.

Enter the cache.

Since caches keep data in memory, they are much more performant than traditional databases. And for social media sites, where 20% of the most popular content drives 80% of the traffic, caching can dramatically decrease the load on the databases.![graph showing SQL retrieval speed with and without a cache](https://miro.medium.com/max/1528/1\*1KVhvYNodqDcOPwqJx89\_g.png)SQL vs. cache speed comparison (Image Credit: [https://dzone.com/articles/redis-vs-mysql-benchmarks](https://dzone.com/articles/redis-vs-mysql-benchmarks))

The next time an interviewer asks you how to optimize an API endpoint or workflow that requires fetching the same data over and over, caching is a good place to start.

However, knowing how caches work and how to use them is easy. Knowing how to build one yourself is the hard part. And that’s exactly what we’ll be focusing on.

## How to Build an LRU Cache <a href="#b102" id="b102"></a>

For the purposes of this article, I will be using Python to implement the LRU cache. It’s succinct, easy to read, and a lot of people know it. However, if Python isn’t your thing or you’re curious about how to implement it in other languages, you can check out my [examples repository](https://github.com/sunny-b/lrucache\_examples).

### Requirements <a href="#4616" id="4616"></a>

Before we start building our cache, we have to understand what the requirements are. The first will be the API. Which methods do we need to implement?

While production quality caches are feature rich, we’ll keep it simple. You’ll only need to create two methods:

* `get(key)`
* `set(key, value)`

But that isn’t all. The LRU cache itself has a number of requirements:

1. When the max size of the cache is reached, remove the least recently used key.
2. Whenever a key is fetched or updated, it becomes the most recently used.
3. Both `get` and `set` operations must complete in [O(1) time complexity](https://stackoverflow.com/questions/697918/what-does-o1-access-time-mean) (meaning that no matter how large the cache is, it takes the same amount of time to complete).
4. When fetching a key that doesn’t exist, return a `null` value.

With these requirements in mind, we can get to work.![person working on a laptop](https://miro.medium.com/max/60/0\*xSMMi7sHX9EfD4eh?q=20)![person working on a laptop](https://miro.medium.com/max/7157/0\*xSMMi7sHX9EfD4eh)Photo by [ConvertKit](https://unsplash.com/@convertkit) on [Unsplash](https://unsplash.com)

### Data structure <a href="#084b" id="084b"></a>

The first question that we need to answer is, which data structure should back our LRU cache? After all, a cache is not a data structure itself.

Since the cache uses `get` and `set` and needs to operate in O(1) time, you might’ve thought of a hash map or dictionary. That would indeed fulfill some requirements. But what about removing the LRU key? A dictionary doesn’t allow us to know which is the oldest key.

We could use a time stamp as part of the dictionary value and update it whenever the key is fetched. This would tell us which key-value pair is the oldest. The problem is, we would need to loop through all the dictionary entries to check which is the oldest — breaking our O(1) requirement.

So what’s the answer?

This is where I should let you in on a secret. We’re actually going to need **two** data structures: one for fetching the values (dictionary/hash map) and one for keeping the items sorted by frequency of use.

### The second data structure <a href="#54ae" id="54ae"></a>

So what should the second data structure be? If you thought of an array, you’re getting close.

We can use an array to keep the items sorted. And each key in the dictionary can reference the index of a value in the array. Whenever a key is fetched, we can move that value to the front of the array, pushing the rest of the items back, and update the index in the dictionary.

However, there’s still a small issue. Under the hood, arrays are a continuous row of data cells. If we need to move values to the front of the array and push the rest down, that operation will get slower as the array increases in size. In other words, inserting items at the beginning of an array takes O(N) time.![diagram showing how arrays need to move cells for insertions](https://miro.medium.com/max/60/1\*suuHOtE36U1NqV9zoigyzg.png?q=20)![diagram showing how arrays need to move cells for insertions](https://miro.medium.com/max/727/1\*suuHOtE36U1NqV9zoigyzg.png)Image Credit: Author

But we are close. We just need a data structure with the same sorting benefits of an array that can also get, set, and delete data in O(1) time. And the data structure that fits all of those requirements is a [doubly linked list (DLL)](https://en.wikipedia.org/wiki/Doubly\_linked\_list).

A doubly linked list is the same as a [linked list](https://www.geeksforgeeks.org/data-structures/linked-list/), except each node in the list has an additional reference to the previous node as well as the next node.![diagram of the structure of a doubly linked list](https://miro.medium.com/max/60/0\*2w-LLnoE6SYOv9a5.png?q=20)![diagram of the structure of a doubly linked list](https://miro.medium.com/max/1440/0\*2w-LLnoE6SYOv9a5.png)Image Credit: [https://medium.com/flawless-app-stories/doubly-linked-lists-swift-4-ae3cf8a5b975](https://medium.com/flawless-app-stories/doubly-linked-lists-swift-4-ae3cf8a5b975)

Each key in the dictionary will reference a node in our linked list. This way, we’ll be able to easily fetch data, as well as update and delete it.![diagram showing the relationship between a dictionary and a doubly linked list](https://miro.medium.com/max/60/1\*J9PdywPRbyMNcztsIsMASg.png?q=20)![diagram showing the relationship between a dictionary and a doubly linked list](https://miro.medium.com/max/1154/1\*J9PdywPRbyMNcztsIsMASg.png)Image Credit: [https://corvostore.github.io/#LRU](https://corvostore.github.io/#LRU)

## Building the LRU Cache <a href="#f650" id="f650"></a>

Now that we know the data structure makeup of our LRU cache, we can start building it!

Looking at this code, you may assume that we’re done. But there’s a catch. Did you notice the `DoublyLinkedList` on Line 6? Python doesn’t come with a `DoublyLinkedList` in the standard library.

In fact, the majority of programming languages don’t come equipped with a DLL. And since live coding challenges rarely let you use third-party libraries, we will have to implement it ourselves.

## Building the Doubly Linked List <a href="#6ace" id="6ace"></a>

When it comes to crafting a DLL, the most important aspect is the design. There are many different types of linked lists with different tradeoffs, but for our use cases, we’ll be making a circular DLL. This will give us quick access to the head and tail, which is important for an LRU cache.![diagram of a circular doubly linked list](https://miro.medium.com/max/1300/1\*myztJG4u2y5oau8\_JcPe4w.png)Circular doubly linked list w/ root node (Image Credit: Author)

While linked lists usually come with many convenience methods, we only need to implement a small subset of them:

1. `move_front(node)`
2. `unshift(value)`
3. `remove_tail`

### MoveFront <a href="#f547" id="f547"></a>

Whenever a key is fetched from our cache, we will need to make that key the most recently used. This involves moving that key to the front of the DLL.

Implementing `move_front` can be a bit confusing as it involves both removing a node from the list and inserting it in a different location. We do this by changing and swapping the `next` and `prev` on a number of surrounding nodes.![Animation showing the effect of MoveFront on a doubly linked list](https://miro.medium.com/freeze/max/60/0\*46btArIaub0qpvcD.gif?q=20)![Animation showing the effect of MoveFront on a doubly linked list](https://miro.medium.com/max/780/0\*46btArIaub0qpvcD.gif)Move Front animation (Image Credit: Author)

### Unshift <a href="#6382" id="6382"></a>

The `unshift` method for a DLL works very similarly to an array where we insert a value at the front or head of the list. Our LRU cache will use this method whenever `set(key, value)` is called.

Luckily for us, `unshift` shares a lot of logic with `move_front`. If we update `move_front` slightly, we can use it to implement `unshift`.![animation of the effect of unshift](https://miro.medium.com/freeze/max/60/1\*Ks8dkzLBMVt7W-KYGzqOvg.gif?q=20)![animation of the effect of unshift](https://miro.medium.com/max/342/1\*Ks8dkzLBMVt7W-KYGzqOvg.gif)Unshift animation (Image Credit: [https://visualgo.net](https://visualgo.net/en/list))

### RemoveTail <a href="#0674" id="0674"></a>

If our cache reaches its max size, we will need to remove the least recently used key. This involves removing the tail of our DLL. Similar to `unshift`, `remove_tail` shares some common logic with `move_front`. While we can’t reuse `move_front` for `remove_tail`, we can abstract some of the common logic.![animation of the effect of Remove Tail](https://miro.medium.com/freeze/max/60/1\*X3FCZSkJrMm6YN7L4uv\_YA.gif?q=20)![animation of the effect of Remove Tail](https://miro.medium.com/max/368/1\*X3FCZSkJrMm6YN7L4uv\_YA.gif)Remove Tail animation (Image Credit: [https://visualgo.net](https://visualgo.net/en/list))

## Putting It All Together <a href="#56d8" id="56d8"></a>

When we combine our LRU cache and DLL, this is what we get:

And just like that, 10 minutes later, we’ve created a basic LRU cache in less than 100 lines of code. For extra practice, you can try adding the ability to delete a key, creating simple unit tests to prove it works, or creating it in a different language.

As I mentioned before, if Python isn’t your thing, don’t worry. [I have more examples for JavaScript, Ruby, and Go](https://github.com/sunny-b/lrucache\_examples). I’m also open to pull requests for other languages.
