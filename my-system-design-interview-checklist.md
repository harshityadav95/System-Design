# My System Design Interview Checklist

### Drive the interview <a href="#3170" id="3170"></a>

Make sure you are the one driving the interview and not your interviewer. This does not mean that you do not let them speak, but rather, you should be the one doing most of the talking, proactively calling out issues in your design before the interviewer points it out, handle the edge cases that the interviewer might poke you on etc.

### FRs and NFRs <a href="#7c04" id="7c04"></a>

Clearly call out the Functional and Non-Functional requirements. The intent is that the requirements should be big enough that makes the problem challenging and also finite enough that you can build a system that fulfills those requirements within the stipulated time. From the Non Functional side, try to make a system that works at a very large scale. What’s the fun in designing a system which works at a low scale?\
Before finalizing the FRs and the NFRs, get them reviewed with your interviewer to make sure they do not want to add/ remove something. At times, interviewers do have some particular use cases in mind that they want to go over.

### Capacity Estimation <a href="#a469" id="a469"></a>

Check with your interviewer if they want to get into this. A lot of people prefer to skip the calculations and focus more on the design, assuming a large enough approximate number for the number of cores required or the amount of disk required etc.

### Plan <a href="#ff63" id="ff63"></a>

Based on the FRs and NFRs, come up with these things:

1. User Interaction Points.
2. Latency/ Availability/ Consistency requirements at each of the user interaction points.
3. A quick analysis estimating if it’s a read-heavy interaction or a write-heavy interaction.
4. Based on the above three, come up with what all services you’ll need and what kind of databases you can use to store the data that each of these services owns.

### HLD <a href="#3314" id="3314"></a>

Come up with a high level component diagram, that covers the following:

1. What all services are present? Make sure you divide the flow into multiple functional components and see if a microservices based approach makes sense or not. Usually, using a microservices based approach is a good idea in SD interviews.
2. How do the services interact with each other and what kind of protocols are used for inter service communication like Async/ Sync — Rest, RPC etc?
3. How would the users interact with the whole system and what all services are user facing. Do you need a Cache to reduce latencies?
4. Which service uses what Database and why? [You can refer to this video that can help you choose the right database based on your use case](https://youtu.be/cODCpXtPHbQ)
5. See if you need caching anywhere and if you do, then what shall be the eviction policy, do you need an expiry for the keys, should it be a write through cache etc?
6. Basis all this analysis, draw out a High Level Diagram of your whole system.

### Must Haves <a href="#0a1c" id="0a1c"></a>

Some key things your high level diagram should have are:

1. Load Balancers
2. Services
3. Databases and Caches
4. User interaction points
5. Any other tools like a Message Queue, CDN, etc.

### Walkthrough the design <a href="#6cfb" id="6cfb"></a>

Once you have the whole diagram ready, go over the whole design, one use case at a time and explain your design to your interviewer at a very high level. Talk about why you have chosen a particular database here and why you have used a particular mode of communication like Sync/ Async etc. You can also get into an RPC vs HTTP kind of a conversation if you made a particular design choice. You should go over what kind of data replication strategy is being used in your databases, for example, would you use a Master-Slave or a Multi Master setup etc.

## User Interface <a href="#afe5" id="afe5"></a>

First things first, how are the users interacting with your system? Is it via a mobile app or on a laptop or a smart TV? This gives us an indication of what kinds of limitations and hidden requirements we are dealing with. For example, in the case of a mobile user, we might have to handle fluctuating networks, for a smart TV and laptop user kind of scenario we might have to support different file formats and different resolutions and aspect ratios.

## Load Balancer <a href="#c29a" id="c29a"></a>

You cannot design a distributed system without a load balancer. Load balancing means distributing the requests between various nodes of a distributed system, so as to decrease response time and improve resource utilization. By distributing the load between various nodes, we rule out the chance of having a single point of failure and also ensure that a single node is not being overloaded while another server might be sitting idle.

## Web Sockets <a href="#f3dd" id="f3dd"></a>

Or to be more specific _web socket handler_ and _web socket manager_. I know, these are not standard terms. But it is something I came across recently on a youtube video and it makes total sense. A **Web Socket Handler** is basically a server that keeps an open web socket connection with all active user devices. Any request from the user will enter the system via a web socket handler and any notifications or responses to the user will be sent via the web socket handler as well. Now if we are working on a large-scale application, with users distributed across various locations, we are going to need web socket handlers distributed across various geographical locations. Think _low latency_. Now users could also switch between various web socket handlers due to let’s say network fluctuations, or simply because they are now physically closer to another web socket handler. In this case, we need a central repository to keep track of which user is connected to which web socket handler. This is done by a **Web Socket Manager**.

## Database <a href="#8aea" id="8aea"></a>

Well, not just database, storage solutions in general. If we need to save images or files, we will need blob storage like Amazon S3, if we are storing payment records, we need it to follow ACID properties and need a relational database in such a case. If we are storing product-related information on a platform like Amazon we will probably need a document DB like Mongo DB. If we were building Twitter or Facebook, we cannot design our system without a Caching solution like Redis. There are so many things we need to consider while deciding which storage solution to use! Here is one of the best articles I came across during my prep -> [Choosing the best Database Solution in a System Design Interview](https://www.codekarle.com/system-design/Database-system-design.html) by CodeKarle.

## CDN <a href="#df49" id="df49"></a>

Again, if our users are distributed geographically, it makes sense to maintain copies of frequently accessed data in a data center closer to the users’ location. For example, a lot of traffic for “Scared Games” on Netflix probably comes from India. So it makes sense to keep the data in a data center close to India to reduce the latency and improve the user experience. Here is a short video by Akamai, explaining briefly exactly what a CDN does -> [What is a CDN](https://youtu.be/l6X\_IxyGHHU)

## Analytics Components <a href="#c8c0" id="c8c0"></a>

Remember, no matter what kind of a system you are creating, there is always scope for analytics. It is one of those things that are never called out during requirement gathering but are always required. This is when we use a stream processing software like Kafka. Whatever goes on in the system, a user logs in or logs out, gets dropped off from a call, cancels an order, cancels a payment, anything! It is all useful information for us! Whenever an event occurs, it should be written to Kafka. On Kafka, we could have some simple spark streaming jobs running for real-time processing. Or we could simply dump the data in a Hadoop cluster and later run some fancy machine learning algorithms for detailed analytics.

## Pluggable components <a href="#dc4b" id="dc4b"></a>

If we were designing Twitter, post length is one of the limitations we need to handle. But what if a user wants to share a link? That would take up half the post! Which is why we need a _URL shortening service_. No matter what system you build, there will always be something or the other that needs to be notified to the user. This is where you might need a _notification service_. Notification services are actually a great way to understand the requirement for pluggability. What if as per our requirement, we need to send in-app notifications if a friend likes a profile picture on Facebook. So we build that into our system. Now a new requirement comes in to send out an email notification to users who haven’t accessed the app in more than 3 days. We can’t add another service to handle email notifications. This is why our notification service needs to be pluggable, so we can add handlers as we go and support new notification methods easily.

Another example of an independent service being used as a separate component would be an _asset delivery service_. If you were building something like Netflix, we need a system to load video content to CDNs depending on the incoming traffic, manage what resolutions and aspect ratio data needs to be sent to which user, takes care of transcoding the main file to all other supported file formats, etc. I will attach a link to one of the videos I referred to, to understand how an asset delivery system might work.

## Monitoring and Alerting <a href="#153b" id="153b"></a>

Are you reaching milestones? Meeting deadlines? Is your system working as expected? Be it any application, no matter how well it is planned or designed, there is always a chance for something to go wrong. A third-party dependency could crash, a server could go down, maybe a server’s CPU utilization is very high while another is underutilized, maybe the system could not scale enough during the Black Friday Sale! Wouldn’t it be great to know these things beforehand?

But how? By monitoring your system of course! And alerting. Anything that is behaving unexpectedly or is reaching its limit, must be called out BEFORE it gets out of hand. This is another one of those things that are not called out initially but are very important while building a system. [Here](https://youtu.be/YyOXt2MEkv4?t=2056) is an example of how you can do that.

## Brownie Points <a href="#7aa8" id="7aa8"></a>

Once you have come up with a solution that meets all the requirements called out in the requirement gathering stage, it is your time to actually shine and stand out from other applicants. This is when you consider **edge cases** that people usually don’t consider in interviews. Like how does Google handle disputed areas like India-Pakistan-China borders in Google Maps? Or you could demonstrate how you can **extend your solution** to support a much larger scale than intended. Like can your video conferencing system be extended to live broadcast an event?

## Resources you might want to look at <a href="#b3a5" id="b3a5"></a>

_What is a load balancer? ->_ [_https://medium.com/@itIsMadhavan/what-is-load-balancer-and-how-it-works-f7796a230034_](https://medium.com/@itIsMadhavan/what-is-load-balancer-and-how-it-works-f7796a230034)

_How to choose the best storage solution in your system design interview? ->_ [_https://www.codekarle.com/system-design/Database-system-design.html_](https://www.codekarle.com/system-design/Database-system-design.html)

_What exactly is a CDN? ->_ [_https://youtu.be/l6X\_IxyGHHU_](https://youtu.be/l6X\_IxyGHHU)

_How to scale your system with Redis ->_ [_https://youtu.be/CtV-QymAeIc_](https://youtu.be/CtV-QymAeIc)

_How an asset delivery system works (with Netflix example) ->_ [_https://youtu.be/lYoSd2WCJTo_](https://youtu.be/lYoSd2WCJTo)

_Here’s another medium article I wrote about choosing the best DB for a system design interview ->_ [_https://towardsdatascience.com/choosing-the-right-database-in-a-system-design-interview-b8af9c6dc525?source=friends\_link\&sk=d78ef7e0b0f4071636c57681b9fce1c8_](https://towardsdatascience.com/choosing-the-right-database-in-a-system-design-interview-b8af9c6dc525?source=friends\_link\&sk=d78ef7e0b0f4071636c57681b9fce1c8)

_A comprehensive guide for cracking the next system design interview by CodeKarle ->_ [_https://www.codekarle.com/_](https://www.codekarle.com)

_Commonly asked system design interview questions ->_ [_https://www.youtube.com/watch?v=EpASu\_1dUdE\&list=PLhgw50vUymyckXl3D1IlXoVl94wknJfUC_](https://www.youtube.com/watch?v=EpASu\_1dUdE\&list=PLhgw50vUymyckXl3D1IlXoVl94wknJfUC)
