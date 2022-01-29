# Distributed Design Patterns



istributed applications are a staple of the modern software development industry. They’re pivotal to cloud storage services and allow web applications of massive scale to stay reactive. As programmers build these systems, they need fundamental building blocks they can use as a starting point and to communicate in a shared vocabulary.

This is where distributed system design patterns become invaluable. While they’re sometimes overused, understanding how to use them is a key skill recruiters are looking for and is essential to stand out in advanced system design interviews.

Today, we’ll explore five of the top distributed system design patterns to help you learn their advantages, disadvantages, and when to use them.

Here’s what we’ll cover today:

* What is a distributed system design pattern?
* 1\. Command and Query Responsibility Segregation
* 2\. Two-Phase Commit
* 3\. Saga
* 4\. Replicated Load-Balanced Services
* 5\. Sharded Services
* What to learn next

## What Is a Distributed System Design Pattern? <a href="#ffd9" id="ffd9"></a>

Design patterns are tried and tested ways of building systems that each fit a particular use case. They’re not implementations but rather abstract ways of structuring a system. Most design patterns have been developed and updated over years by many different developers, meaning they’re often very efficient starting points.

Design patterns are building blocks that allow programmers to pull from existing knowledge rather than starting from scratch with every system. They also create a set of standard models for system design that help other developers see how their projects can interface with a given system.

Creational design patterns provide a baseline when building new objects. Structural patterns define the overall structure of a solution. Behavioral patterns describe objects and how they communicate with each other.

Distributed system design patterns are design patterns used when developing distributed systems, which are essentially collections of computers and data centers that act as one computer for the end-user. These distributed design patterns outline a software architecture for how different nodes communicate with each other, which nodes handle each task, and the process flow for different tasks.

These patterns are widely used when designing the distributed system architecture of large-scale cloud computing and scalable microservice software systems.

### Types of distributed design patterns <a href="#cc12" id="cc12"></a>

Most distributed design patterns fall into one of three categories based on the functionality they work with:

* Object communication: Describes the messaging protocols and permissions for different components of the system to communicate.
* Security: Handles confidentiality, integrity, and availability concerns to ensure the system is secure from unauthorized access.
* Event-driven: Patterns that describe the production, detection, consumption, and response to system events.

## 1. Command and Query Responsibility Segregation (CQRS) <a href="#a217" id="a217"></a>

The CQRS pattern focuses on separating the read and write operations of a distributed system to increase scalability and security. This model uses commands to write data to persistent storage and queries to locate and fetch the data.

These are handled by a command center that receives requests from users. The command center then fetches the data and makes any necessary modifications, saves the data, and notifies the read service. The read service then updates the read model to show the change to the user.

### Advantages <a href="#ac4c" id="ac4c"></a>

* Reduces system complexity by delegating tasks.
* Enforces a clear separation between business logic and validation.
* Helps categorize processes by their job.
* Reduces the number of unexpected changes to shared data.
* Reduces the number of entities that have modifying access to data.

### Disadvantages <a href="#d05c" id="d05c"></a>

* Requires constant back-and-forth communication between command and read models.
* Can cause increased latency when sending high-throughput queries.
* No means to communicate between service processes.

### **Use case** <a href="#0735" id="0735"></a>

CQRS is best for data-intensive applications like SQL or NoSQL database management systems. It’s also helpful for data-heavy microservice architectures. It’s great for handling stateful applications because the writer/reader distinction helps with immutable states.

## 2. Two-Phase Commit (2PC) <a href="#6627" id="6627"></a>

2PC is similar to CQRS in its transactional approach and reliance on a central command, but partitions are processed by their type and what stage of completion they’re on. The two phases are the _Prepare_ phase (in which the central control tells the services to prepare the data) and the _Commit_ phase (which signals the service to send the prepared data).

All services in a 2PC system are locked by default, meaning they cannot send data. While locked, services complete the Prepare stage so they’re ready to send once unlocked. The coordinator unlocks services one by one and requests their data. If the service is not ready to submit its data, the coordinator moves on to another service. Once all prepared data has been sent, all services unlock to await new tasks from the coordinator.

2PC essentially ensures that only one service can operate at a time, which makes the process more resistant and consistent than CQRS.

### Advantages <a href="#41dd" id="41dd"></a>

* Consistent and resistant to errors due to lack of concurrent requests.
* Scalable — can handle big data pools as easily as it can handle data from a single machine.
* Allows for isolation and data sharing at the same time.

### Disadvantages <a href="#de44" id="de44"></a>

* Not fault-tolerant, prone to bottlenecks and blocking due to its synchronous nature.
* Requires more resources than other design patterns.

### **Use case** <a href="#350c" id="350c"></a>

2PC is best for distributed systems that deal with high-stakes transaction operations that favor accuracy over resource efficiency. It is resistant to error and it is easy to track mistakes when they occur, even at scale.

## 3. Saga <a href="#3eca" id="3eca"></a>

Saga is an asynchronous pattern that does not use a central controller and instead communicates entirely between services. This overcomes some of the disadvantages of the previously covered synchronous patterns.

Saga uses an Event Bus to allow services to communicate with each other in a microservice system. The bus sends and receives requests between services, and each participating service creates a local transaction. The participating services then each emit an event for other services to receive. Other services all listen for events. The first service to receive the event will perform the required action. If that service fails to complete the action, it’s sent to other services.

This structure is similar to the 2PC design in that services are cycled if one cannot complete a task. However, Saga removes the central control element to better manage the flow and reduce the amount of back-and-forth communication required.

### **Advantages** <a href="#8a0d" id="8a0d"></a>

* Individual services can handle much longer transactions.
* Great for the distributed system due to decentralization.
* Reduces bottlenecks thanks to peer-to-peer communication between services.

### **Disadvantages** <a href="#f02c" id="f02c"></a>

* Asynchronous autonomy makes it difficult to track which services are doing individual tasks.
* Difficult to debug due to complex orchestration.
* Less service isolation than previous patterns.

### **Use case** <a href="#05d5" id="05d5"></a>

Saga’s decentralized approach is great for scalable serverless functions that handle many parallel requests at once. AWS uses Saga-based designs in many functions like step and lambda functions.

## 4. Replicated Load-Balanced Services (RLBS) <a href="#6ddd" id="6ddd"></a>

The RLBS pattern is the simplest and most commonly used design pattern. At the most basic level, it consists of multiple identical services that all report to a central load balancer. Each service is capable of handling tasks and can replicate if they fail. The load balancer receives requests from the end-user and distributes them to the services either in a round-robin fashion or sometimes by using a more complex routing algorithm.

The duplicate services ensure the application maintains a high availability for user requests and can redistribute work if one instance of the service should fail.

RLBS is often used with Azure Kubernetes, which is an open-source container orchestration technology made by Microsoft that offers automatic service scaling based on workflow.

### **Advantages** <a href="#00ed" id="00ed"></a>

* Consistent performance from the view of the end-user.
* Can quickly recover from failed services.
* Highly scalable with more services.
* Excellent for concurrency.

### **Disadvantages** <a href="#9dda" id="9dda"></a>

* Inconsistent performance based on load balancer algorithm.
* Resource-intensive to manage services.

### **Use case** <a href="#1c48" id="1c48"></a>

RLBS is great for front-facing systems that have inconsistent workloads throughout the day but must maintain low latency, such as entertainment web apps like Netflix or Amazon Prime.

## 5. Sharded Services <a href="#36d8" id="36d8"></a>

An alternative to replica-based designs is to create a selection of services that each only completes a certain kind of request. This is called “sharding” because you split the request flow into multiple unequal sections. For example, you may have one shard service that accepts all caching requests and another that only handles high-priority requests. The load balancer evaluates each request when it comes in and distributes it to the appropriate shard for completion.

Sharded services are normally used for building stateful services because the size of the state is often too large for a single stateless container. Sharding lets you scale the individual shard to meet the size of the state.

Sharded services also allow you to handle high-priority requests faster. Shards dedicated to high-priority requests are always available to handle such requests the moment they come in rather than having them placed in the queue.

### **Advantages** <a href="#dfb9" id="dfb9"></a>

* Allows you to scale shards for common requests.
* Easy to prioritize requests.
* Simple to debug due to natural sorting.

### **Disadvantages** <a href="#409e" id="409e"></a>

* Can be resource-intensive to maintain many shards.
* Leads to loss in performance if shards are used disproportionately.

### **Use case** <a href="#400d" id="400d"></a>

Sharded services are best when your system receives a predictable imbalance in request types, but some requests have priority.

## What To Learn Next <a href="#e097" id="e097"></a>

Distributed system design patterns are an essential part of any successful backend system. However, these are just a few of the patterns used by professional software engineers.

Some patterns for you to learn next are:

* Sidecar Pattern
* Write-Ahead Log
* Split-Brain Pattern
* Hinted Handoff
* Read Repair
