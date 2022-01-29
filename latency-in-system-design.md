# Latency in System Design



## What is Latency? <a href="#5c62" id="5c62"></a>

Latency is the time interval between the start of the request at the client end to deliver the result back to the client by the server, i.e., the round trip time between the browser and the server.

Our ultimate goal would be to develop a latency-free system, but various bottlenecks prevent developing such systems in the real world. Lower the latency of the system, the less time it takes to process our requests. Every time whenever a request is made, the browser sends a signal to the server. Servers process the requests and retrieve the information to bring it back to the client.

## How does latency work? <a href="#518d" id="518d"></a>

Latency is nothing else than the estimated time the client has to wait for after starting the request to receive the result. Let’s take an example and look into how it works.

Suppose you interact with an e-commerce website, say, Walmart, and you liked something and added it to the cart. Now when you press the “Add to Cart” button, the following events will happen:

1. The instant the “Add to Cart” button was pressed, the clock for latency starts, and the browser initiate a request to the servers.
2. The server acknowledges the request and processes it.
3. The server replies to the request, and the request reaches your browser, and the product gets added to your Cart.

You can start the stopwatch in 1st step and stop the stopwatch in the last step, and the diff would be the latency.

## What causes latency? <a href="#cc37" id="cc37"></a>

Now, you must have got the gist of the idea, but do you know where the latency comes from? The latency in a network depends on various parameters, and they have a sound effect in determining its value. One of the major factors contributing to latency is outbound calls. In the previous example of adding cart exercise, when you click the button on the browser, the request goes to some server in the backend, which again calls multiple services internally for computation (in parallel or sequentially) and then waits for a response or aggregates them. All this adds to the latency of the call. However, It is mainly caused by the following factors:

**Transmission mediums:** The transmission medium is the physical path between the start and the endpoint. The Latency of the system Latency depends on the type of medium used for the transmission of requests. Transmission mediums like WAN and Fiber Optic Cables are widely used, but each medium comes up with its limitations, affecting the latency.

**Propagation:** It is referred to the amount of time required for a packet to travel from one source to another. The latency of the system is highly dependent on the distance between the communicating nodes. The farther the nodes are located, the more the latency of the system.

**Routers:** Routers form an essential component in communication and take some time to analyze the header information of a packet. The latency depends on how efficiently the router processes the request. Router to router hop increases the latency of the system.

**Storage delays:** The system’s latency also depends on the kind of storage system used, as it may take some time to process and return data. Hence accessing stored data can increase the latency of the system.

## How to measure latency? <a href="#1bee" id="1bee"></a>

There are many methods used to quantify latency. We can measure it in different ways; the three most common methods are:

**Ping:** Ping is the most common utility used to measure latency. It sends packets to an address and sees how fast the response is coming. Ping measures how long it takes for the data to travel from source to destination and back to the source. A faster ping corresponds to a more responsive connection.

**Traceroute:** Traceroute is another utility used to test latency. It also uses packets to calculate the time taken for each hop when routed to the destination.

**MTR:** MTR is a combination of both ping and Traceroute. MTR gives a report that lists how each hop in a network is required for a packet to travel from one end to the other. The report generally includes various details such as percentage Loss, Average Latency, etc.

## Latency Optimization <a href="#3f5e" id="3f5e"></a>

Latency restricts the performance of the system; hence it is necessary to optimize it. We can reduce it by adopting the following measures:

**HTTP/2:** We can reduce it by the use of HTTP/2. It allows parallelized transfers and minimizes the round trips from the sender to the receiver, which are highly effective in reducing the latency.

**Less external HTTP requests:** Latency increases because of third-party services. By reducing the number of external HTTP requests, the system’s latency gets optimized as third-party services affect both the application’s speed and quality.

**CDN:** CDNs proved to be a boon in reducing latency. CDN caches the resources in multiple locations worldwide and reduces the travel time of the request and response. Hence instead of going back to the origin server now, the request can be fetched using the cached resources closer to the clients.

**Browser Caching:** Browser Caching can also help reduce the latency by caching specific resources locally to decrease the number of requests made to the server.

**Disk I/O:** The goal is to optimize algorithms to minimize the impact of disk I/O. Hence, instead of often writing to disk, use write-through caches or in-memory databases or combine writes where possible or use fast storage systems, such as SSDs.

As a developer, latency can also be optimized by making smarter choices regarding storage layer, data modeling, outbound call fanout, etc. Here are some ways to optimize it at an application level:

* Inefficient algorithms are the most apparent sources of latency in code. It is necessary to avoid unnecessary loops or nested expensive operations.
* Use design patterns that avoid locking as multithreaded locks introduce latency.
* Use an asynchronous programming model to utilize better hardware resources as blocking operations cause long wait times.
* Limiting the unbounded queue depths and providing back pressure typically lead to less wait time in the code resulting in more predictable latencies.

