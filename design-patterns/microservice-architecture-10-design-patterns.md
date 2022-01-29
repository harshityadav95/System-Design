---
description: >-
  Microservice Architecture, Database per Microservice, Event Sourcing, CQRS,
  Saga, BFF, API Gateway, Strangler, Circuit Breaker, Externalize Configuration,
  Consumer-Driven Contract Testing
---

# Microservice Architecture 10 Design Patterns

Tackling complexity in large Software Systems was always a daunting task since the early days of Software development (1960's). Over the years, Software Engineers and Architects made many attempts to tackle the complexities of Software Systems: [**Modularity and Information Hiding**](https://www.win.tue.nl/\~wstomv/edu/2ip30/references/criteria\_for\_modularization.pdf) **** by [_**David Parnas**_](https://en.wikipedia.org/wiki/David\_Parnas) _****_ (1972), [**Separation of Concern**](https://www.cs.utexas.edu/users/EWD/transcriptions/EWD04xx/EWD447.html) by [_**Edsger W. Dijkstra**_](https://en.wikipedia.org/wiki/Edsger\_W.\_Dijkstra) _****_ (1974)_**,**_ [**Service Oriented Architecture**](https://en.wikipedia.org/wiki/Service-oriented\_architecture) **** (1998).

All of them used the age-old and proven technique to tackle the complexity of a large system: **divide and conquer**. Since the 2010s, those techniques proved insufficient to tackle the complexities of Web-Scale applications or modern large-scale Enterprise applications. As a result, Architects and Engineers developed a new approach to tackle the complexity of Software Systems in modern times: **Microservice Architecture**. It also uses the same old “Divide and Conquer” technique, albeit in a novel way.

[**Software Design Patterns**](https://en.wikipedia.org/wiki/Software\_design\_pattern) are general, reusable solutions to the commonly occurring problem in Software Design. Design Patterns help us share a common vocabulary and use a battle-tested solution instead of reinventing the wheel. In a previous article: [**Effective Microservices: 10 Best Practices**](https://towardsdatascience.com/effective-microservices-10-best-practices-c6e4ba0c6ee2), I have described a set of best practices to develop Effective Microservices. Here, I will describe a set of Design Patterns to help you implement those best practices. If you are new to Microservice Architecture, then no worries, I will introduce you to Microservice Architecture.

By reading this article, you will learn:

* Microservice Architecture
* Advantages of Microservice Architecture
* Disadvantages of Microservice Architecture
* When to use Microservice Architecture
* The Most important Microservice Architecture Design Patterns, including their advantages, disadvantages, use cases, Context, Tech Stack example, and useful resources.

Please note that most of the Design Patterns of this listing have several contexts and can be used in non-Microservice Architecture. But I will describe them in the context of Microservice Architecture.

## Microservice Architecture <a href="#3947" id="3947"></a>

I have covered Microservice Architecture in details in my previous Blog Posts: [**Microservice Architecture: A brief overview and why you should use it in your next project**](https://towardsdatascience.com/microservice-architecture-a-brief-overview-and-why-you-should-use-it-in-your-next-project-a17b6e19adfd) **** and [**Is Modular Monolithic Software Architecture Really Dead?**](https://towardsdatascience.com/looking-beyond-the-hype-is-modular-monolithic-software-architecture-really-dead-e386191610f8)**.** If you are interested, then you can read them to have a deeper look.

What is a Microservice Architecture. There are many definitions of Microservice Architecture. [Here is my definition](https://towardsdatascience.com/looking-beyond-the-hype-is-modular-monolithic-software-architecture-really-dead-e386191610f8):

> _**Microservice Architecture is about splitting a large, complex systems vertically (per functional or business requirements) into smaller sub-systems which are processes (hence independently deployable) and these sub-systems communicates with each other via lightweight, language-agnostic network calls either synchronous (e.g. REST, gRPC) or asynchronous (via Messaging) way.**_

Here is the Component View of a Business Web Application with Microservice Architecture:![](https://miro.medium.com/max/60/0\*W5T2tfcKgudQZYnu.jpeg?q=20)![](https://miro.medium.com/max/883/0\*W5T2tfcKgudQZYnu.jpeg)[Microservice Architecture](https://towardsdatascience.com/looking-beyond-the-hype-is-modular-monolithic-software-architecture-really-dead-e386191610f8) by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

**Important Characteristics of Microservice Architecture:**

* The whole application is split into separate processes where each process can contain multiple internal modules.
* Contrary to Modular Monoliths or SOA, a Microservice application is split vertically **** (according to business capability or domains)
* The Microservice boundary is external. As a result, Microservices communicates with each other via network calls (RPC or message).
* As Microservices are independent processes, they can be deployed independently.
* They communicate in a lightweight way and don’t need any smart Communication channel.

**Advantages of Microservice Architecture:**

* Better development scaling.
* Higher development velocity.
* Supports iterative or incremental modernization.
* Take advantage of the modern Software Development Ecosystem (Cloud, Containers, DevOps, Serverless).
* Supports horizontal scaling and granular scaling.
* It puts low cognitive complexity on the developer’s head thanks to its smaller size.

**Disadvantages of Microservice Architecture:**

* A higher number of Moving parts (Services, Databases, Processes, Containers, Frameworks).
* Complexity moves from Code to the Infrastructure.
* The proliferation of RPC calls and network traffic.
* Managing the security of the complete system is challenging.
* Designing the entire system is harder.
* Introduce complexities of Distributed Systems.

**When to use Microservice Architecture:**

* Web-Scale Application development.
* Enterprise Application development when multiple teams work on the application.
* Long-term gain is preferred over short-term gain.
* The team has Software Architects or Senior Engineers capable of designing Microservice Architecture.

## Design Patterns for Microservice Architecture <a href="#c193" id="c193"></a>

### Database per Microservice <a href="#5dff" id="5dff"></a>

Once a company replaces the large monolithic system with many smaller microservices, the most important decision it faces is regarding the Database. In a monolithic architecture, a large, central database is used. Many architects favor keeping the database as it is, even when they move to microservice architecture. While it gives some short-term benefit, it is an anti-pattern, especially in a large-scale system, as the microservices will be tightly coupled in the database layer. The whole object of moving to microservice will fail (e.g., team empowerment, independent development).

A better approach is to provide every Microservice its own Data store, so that there is no strong-coupling between services in the database layer. Here I am using the term database to show a logical separation of data, i.e., the Microservices can share the same physical database, but they should use separate Schema/collection/table. It will also ensure that the Microservices are correctly segregated according to the [Domain-Driven-Design](https://en.wikipedia.org/wiki/Domain-driven\_design).![](https://miro.medium.com/max/60/1\*WWJQH50jxgrqh-ABFRQuzQ.jpeg?q=20)![](https://miro.medium.com/max/661/1\*WWJQH50jxgrqh-ABFRQuzQ.jpeg)Database per Microservice by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

**Pros**

* Complete ownership of Data to a Service.
* Loose coupling among teams developing the services.

**Cons**

* Sharing data among services becomes challenging.
* Giving application-wide ACID transactional guarantee becomes a lot harder.
* Decomposing the Monolith database to smaller parts need careful design and is a challenging task.

**When to use Database per Microservice**

* In large-scale enterprise applications.
* When the team needs complete ownership of their Microservices for development scaling and development velocity.

**When not to use Database per Microservice**

* In small-scale applications.
* If one team develops all the Microservices.

**Enabling Technology Examples**

All SQL, NoSQL databases offer logical separation of data (e.g., separate tables, collections, schemas, databases).

**Further Reading**[Microservices Pattern: Database per serviceLet's imagine you are developing an online store application using the Microservice architecture pattern. Most services…microservices.io](https://microservices.io/patterns/data/database-per-service.html)[Distributed dataAs we've seen throughout this book, a cloud-native approach changes the way you design, deploy, and manage…docs.microsoft.com](https://docs.microsoft.com/en-us/dotnet/architecture/cloud-native/distributed-data)

### Event Sourcing <a href="#0735" id="0735"></a>

In a Microservice Architecture, especially with **Database per Microservice,** the Microservices need to exchange data. For resilient, highly scalable, and fault-tolerant systems, they should communicate asynchronously by exchanging Events. In such a case, you may want to have Atomic operations, e.g., update the Database and send the message. If you have SQL databases and want to have distributed transactions for a high volume of data, you cannot use the [two-phase locking](https://en.wikipedia.org/wiki/Two-phase\_locking) (2PL) as it does not scale. If you use NoSQL Databases and want to have a distributed transaction, you cannot use 2PL as many NoSQL databases do not support two-phase locking.

In such scenarios, use Event based Architecture with Event Sourcing. In traditional databases, the Business Entity with the current “state” is directly stored. In Event Sourcing, any state-changing event or other significant events are stored instead of the entities. It means the modifications of a Business Entity is saved as a series of immutable events. The State of a Business entity is deducted by reprocessing all the Events of that Business entity at a given time. Because data is stored as a series of events rather than via direct updates to data stores, various services can replay events from the event store to compute the appropriate state of their respective data stores.![](https://miro.medium.com/max/60/1\*tRDaroNg\_GnGdCZDFxLFKQ.jpeg?q=20)![](https://miro.medium.com/max/981/1\*tRDaroNg\_GnGdCZDFxLFKQ.jpeg)Event Sourcing by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

**Pros**

* Provide atomicity to highly scalable systems.
* Automatic history of the entities, including time travel functionality.
* Loosely coupled and event-driven Microservices.

**Cons**

* Reading entities from the Event store becomes challenging and usually need an additional data store (**CQRS** pattern)
* The overall complexity of the system increases and usually need [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven\_design).
* The system needs to handle duplicate events (idempotent) or missing events.
* Migrating the Schema of events becomes challenging.

**When to use Event Sourcing**

* Highly scalable transactional systems with SQL Databases.
* Transactional systems with NoSQL Databases.
* Highly scalable and resilient Microservice Architecture.
* Typical Message Driven or Event-Driven systems (e-commerce, booking, and reservation systems).

**When not to use Event Sourcing**

* Lowly scalable transactional systems with SQL Databases.
* In simple Microservice Architecture where Microservices can exchange data synchronously (e.g., via API).

**Enabling Technology Examples**

_Event Store:_ [EventStoreDB](https://www.eventstore.com), [Apache Kafka](https://kafka.apache.org), [Confluent Cloud](https://www.confluent.io/confluent-cloud), [AWS Kinesis](https://aws.amazon.com/kinesis/), [Azure Event Hub](https://azure.microsoft.com/en-us/services/event-hubs/), [GCP Pub/Sub](https://cloud.google.com/pubsub), [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction), [MongoDB](https://www.mongodb.com), [Cassandra](https://cassandra.apache.org). [Amazon DynamoDB](https://aws.amazon.com/dynamodb/?trk=ps\_a134p000004f2XeAAI\&trkCampaign=acq\_paid\_search\_brand\&sc\_channel=PS\&sc\_campaign=acquisition\_EMEA\&sc\_publisher=Google\&sc\_category=Database\&sc\_country=EMEA\&sc\_geo=EMEA\&sc\_outcome=acq\&sc\_detail=amazon%20dynamodb\&sc\_content=DynamoDB\_e\&sc\_matchtype=e\&sc\_segment=468764879940\&sc\_medium=ACQ-P|PS-GO|Brand|Desktop|SU|Database|DynamoDB|EMEA|EN|Text|xx|EU\&s\_kwcid=AL!4422!3!468764879940!e!!g!!amazon%20dynamodb\&ef\_id=CjwKCAiAq8f-BRBtEiwAGr3DgRRqVmhD5PL323QFmdBJvvOwzxU1nvrGFdbM8ra-DQViD8jjGn-PGBoCWJYQAvD\_BwE:G:s\&s\_kwcid=AL!4422!3!468764879940!e!!g!!amazon%20dynamodb),

_Frameworks:_ [Lagom](https://www.lagomframework.com), [Akka](https://akka.io), [Spring](https://spring.io), [akkatecture](https://akkatecture.net), [Axon](https://axoniq.io), [Eventuate](https://eventuate.io)

**Further Reading**[Event SourcingThe fundamental idea of Event Sourcing is that of ensuring every change to the state of an application is captured in…martinfowler.com](https://martinfowler.com/eaaDev/EventSourcing.html)[Event Sourcing pattern - Cloud Design PatternsInstead of storing just the current state of the data in a domain, use an append-only store to record the full series…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)[Microservices Pattern: Event sourcingA service command typically needs to update the database and send messages/events. For example, a service that…microservices.io](https://microservices.io/patterns/data/event-sourcing.html)

### Command Query Responsibility Segregation (CQRS) <a href="#6a5c" id="6a5c"></a>

If we use Event Sourcing, then reading data from the Event Store becomes challenging. To fetch an entity from the Data store, we need to process all the entity events. Also, sometimes we have different consistency and throughput requirements for reading and write operations.

In such use cases, we can use the CQRS pattern. In the CQRS pattern, the system's data modification part (Command) is separated from the data read (Query) part. CQRS pattern has two forms: simple and advanced, which lead to some confusion among the software engineers.

In its simple form, distinct entity or ORM models are used for Reading and Write, as shown below:![](https://miro.medium.com/max/40/1\*l8GFDOUlyR\_Km0dqg1VTgw.jpeg?q=20)![](https://miro.medium.com/max/341/1\*l8GFDOUlyR\_Km0dqg1VTgw.jpeg)CQRS (simple) by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

It helps to enforce the Single Responsibility Principle and Separation of Concern, which lead to a cleaner design.

In its advanced form, different data stores are used for reading and write operations. The advanced CQRS is used with Event Sourcing. Depending on the use case, different types of Write Data Store and Read Data store are used. The Write Data Store is the “System of Records,” i.e., the entire system's golden source.![](https://miro.medium.com/max/52/1\*wCrXIQ7MD\_20yKmZIBxHvQ.jpeg?q=20)![](https://miro.medium.com/max/435/1\*wCrXIQ7MD\_20yKmZIBxHvQ.jpeg)CQRS (advanced) by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

For the Read-heavy applications or Microservice Architecture, OLTP database (any SQL or NoSQL database offering ACID transaction guarantee) or Distributed Messaging Platform is used as Write Store. For the Write-heavy applications (high write scalability and throughput), a horizontally write-scalable database is used (public cloud global Databases). The normalized data is saved in the Write Data Store.

NoSQL Database optimized for searching (e.g., Apache Solr, Elasticsearch) or reading (Key-Value data store, Document Data Store) is used as Read Store. In many cases, read-scalable SQL databases are used where SQL query is desired. The denormalized and optimized data is saved in the Read Store.

Data is copied from the Write store to the read store asynchronously. As a result, the Read Store lags the Write store and is **Eventual Consistent**.

### **Pros** <a href="#891e" id="891e"></a>

* Faster reading of data in Event-driven Microservices.
* High availability of the data.
* Read and write systems can scale independently.

**Cons**

* Read data store is weakly consistent (eventual consistency)
* The overall complexity of the system increases. Cargo culting CQRS can significantly jeopardize the complete project.

**When to use CQRS**

* In highly scalable Microservice Architecture where event sourcing is used.
* In a complex domain model where reading data needs query into multiple Data Store.
* In systems where read and write operations have a different load.

**When not to use CQRS**

* In Microservice Architecture, where the volume of events is insignificant, taking the Event Store snapshot to compute the Entity state is a better choice.
* In systems where read and write operations have a similar load.

**Enabling Technology Examples**

_Write Store:_ [EventStoreDB](https://www.eventstore.com), [Apache Kafka](https://kafka.apache.org), [Confluent Cloud](https://www.confluent.io/confluent-cloud), [AWS Kinesis](https://aws.amazon.com/kinesis/), [Azure Event Hub](https://azure.microsoft.com/en-us/services/event-hubs/), [GCP Pub/Sub](https://cloud.google.com/pubsub), [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction), [MongoDB](https://www.mongodb.com), [Cassandra](https://cassandra.apache.org). [Amazon DynamoDB](https://aws.amazon.com/dynamodb/)

_Read Store:_ [Elastic Search](https://www.elastic.co), [Solr](https://lucene.apache.org/solr/features.html), [Cloud Spanner](https://cloud.google.com/spanner), [Amazon Aurora](https://aws.amazon.com/rds/aurora/), [Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction), [Neo4j](https://neo4j.com)

_Frameworks:_ [Lagom](https://www.lagomframework.com), [Akka](https://akka.io), [Spring](https://spring.io), [akkatecture](https://akkatecture.net), [Axon](https://axoniq.io), [Eventuate](https://eventuate.io)

**Further Reading**[bliki: CQRSCQRS stands for Command Query Responsibility Segregation. It's a pattern that I first heard described by Greg Young. At…martinfowler.com](https://martinfowler.com/bliki/CQRS.html)[CQRS pattern - Azure Architecture CenterThe Command and Query Responsibility Segregation (CQRS) pattern separates read and update operations for a data store…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)[Microservices Pattern: Command Query Responsibility Segregation (CQRS)You have applied the Microservices architecture pattern and the Database per service pattern. As a result, it is no…microservices.io](https://microservices.io/patterns/data/cqrs.html)

### Saga <a href="#9bbd" id="9bbd"></a>

If you use Microservice Architecture with **Database per Microservice**, then managing consistency via distributed transactions is challenging. You cannot use the traditional [Two-phase commit protocol](https://en.wikipedia.org/wiki/Two-phase\_commit\_protocol) as it either does not scale (SQL Databases) or is not supported (many NoSQL Databases).

You can use the Saga pattern for distributed transactions in Microservice Architecture. Saga is an old pattern developed in 1987 as a conceptual alternative for long-running database transactions in SQL databases. But a modern variation of this pattern works amazingly for the distributed transaction as well. Saga pattern is a local transaction sequence where each transaction updates data in the Data Store within a single Microservice and publishes an Event or Message. The first transaction in a saga is initiated by an external request (Event or Action). Once the local transaction is complete (data is stored in Data Store, and message or event is published), the published message/event triggers the next local transaction in the Saga.![](https://miro.medium.com/max/60/1\*-Ro9jCnA7e0PnjnjEa0FWA.jpeg?q=20)![](https://miro.medium.com/max/661/1\*-Ro9jCnA7e0PnjnjEa0FWA.jpeg)Saga by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

If the local transaction fails, Saga executes a series of compensating transactions that undo the preceding local transactions' changes.

There are mainly two variations of Saga transactions co-ordinations:

* _Choreography_: Decentralised co-ordinations where each Microservice produces and listen to other Microservice’s events/messages and decides if an action should be taken or not.
* _Orchestration_: Centralised co-ordinations where an Orchestrator tells the participating Microservices which local transaction needs to be executed.

### Pros <a href="#6329" id="6329"></a>

* Provide consistency via transactions in a highly scalable or loosely coupled, event-driven Microservice Architecture.
* Provide consistency via transactions in Microservice Architecture where NoSQL databases without 2PC support are used.

**Cons**

* Need to handle transient failures and should provide idempotency.
* Hard to debug, and the complexity grows as the number of Microservices increase.

**When to use Saga**

* In highly scalable, loosely coupled Microservice Architecture where event sourcing is used.
* In systems where distributed NoSQL databases are used.

**When not to use Saga**

* Lowly scalable transactional systems with SQL Databases.
* In systems where cyclic dependency exists among services.

**Enabling Technology Examples**

[Axon](https://axoniq.io), [Eventuate](https://eventuate.io), [Narayana](https://narayana.io)

**Further Reading**[Saga distributed transactions - Azure Design PatternsThe saga design pattern is a way to manage data consistency across microservices in distributed transaction scenarios…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/saga/saga)[Microservices Pattern: SagasYou have applied the Database per Service pattern. Each service has its own database. Some business transactions…microservices.io](https://microservices.io/patterns/data/saga.html)[Saga Pattern: Application Transactions Using MicroservicesTransactions are an essential part of applications. Without them, it would be impossible to maintain data consistency…blog.couchbase.com](https://blog.couchbase.com/saga-pattern-implement-business-transactions-using-microservices-part/)

### Backends for Frontends (BFF) <a href="#232b" id="232b"></a>

In modern business application developments and especially in Microservice Architecture, the Frontend and the Backend applications are decoupled and separate Services. They are connected via API or GraphQL. If the application also has a Mobile App client, then using the same backend Microservice for both the Web and the Mobile client becomes problematic. The Mobile client's API requirements are usually different from Web client as they have different screen size, display, performance, energy source, and network bandwidth.

Backends for Frontends pattern could be used in scenarios where each UI gets a separate backend customized for the specific UI. It also provides other advantages, like acting as a Facade for downstream Microservices, thus reducing the chatty communication between the UI and downstream Microservices. Also, in a highly secured scenario where downstream Microservices are deployed in a DMZ network, the BFF’s are used to provide higher security.![](https://miro.medium.com/max/56/1\*FCZRcAuSLhrNOjcq1zYXDw.jpeg?q=20)![](https://miro.medium.com/max/661/1\*FCZRcAuSLhrNOjcq1zYXDw.jpeg)Backends for Frontends by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

### Pros <a href="#a0ac" id="a0ac"></a>

* Separation of Concern between the BFF’s. We can optimize them for a specific UI.
* Provide higher security.
* Provide less chatty communication between the UI’s and downstream Microservices.

**Cons**

* Code duplication among BFF’s.
* The proliferation of BFF’s in case many other UI’s are used (e.g., Smart TV, Web, Mobile, Desktop).
* Need careful design and implementation as BFF’s should not contain any business logic and should only contain client-specific logic and behavior.

**When to use Backends for Frontends**

* If the application has multiple UIs with different API requirements.
* If an extra layer is needed between the UI and Downstream Microservices for Security reasons.
* If Micro-frontends are used in UI development.

**When not to use Backends for Frontends**

* If the application has multiple UI, but they consume the same API.
* If Core Microservices are not deployed in DMZ.

**Enabling Technology Examples**

Any Backend frameworks (Node.js, Spring, Django, Laravel, Flask, Play, …..) supports it.

**Further Reading**[Sam Newman - Backends For FrontendsWith the advent and success of the web, the de facto way of delivering user interfaces has shifted from thick-client…samnewman.io](https://samnewman.io/patterns/architectural/bff/)[Backends for Frontends pattern - Cloud Design PatternsCreate separate backend services to be consumed by specific frontend applications or interfaces. This pattern is useful…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/patterns/backends-for-frontends)[Microservices Pattern: API gateway patternLet's imagine you are building an online store that uses the Microservice architecture pattern and that you are…microservices.io](https://microservices.io/patterns/apigateway.html)

### API Gateway <a href="#8ac7" id="8ac7"></a>

In Microservice Architecture, the UI usually connects with multiple Microservices. If the Microservices are finely grained (FaaS), the Client may need to connect with lots of Microservices, which becomes chatty and challenging. Also, the services, including their APIs, can evolve. Large enterprises will like to have other cross-cutting concerns (SSL termination, authentication, authorization, throttling, logging, etc.).

One possible way to solve these issues is to use API Gateway. API Gateway sits between the Client APP and the Backend Microservices and acts as a facade. It can work as a reverse proxy, routing the Client request to the appropriate Backend Microservice. It can also support the client request's fanning-out to multiple Microservices and then return the aggregated responses to the Client. It additionally supports essential cross-cutting concerns.![](https://miro.medium.com/max/60/1\*e3p0KRmyiqgEx9kYH1e0Hg.jpeg?q=20)![](https://miro.medium.com/max/711/1\*e3p0KRmyiqgEx9kYH1e0Hg.jpeg)API Gateway by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

### Pros <a href="#9b7a" id="9b7a"></a>

* Offer loose coupling between Frontend and backend Microservices.
* Reduce the number of round trip calls between Client and Microservices.
* High security via SSL termination, Authentication, and Authorization.
* Centrally managed cross-cutting concerns, e.g., Logging and Monitoring, Throttling, Load balancing.

**Cons**

* Can lead to a single point of failure in Microservice Architecture.
* Increased latency due to the extra network call.
* If it is not scaled, they can easily become the bottleneck to the whole Enterprise.
* Additional maintenance and development cost.

**When to use API Gateway**

* In complex Microservice Architecture, it is almost mandatory.
* In large Corporations, API Gateway is compulsory to centralize security and cross-cutting concerns.

**When not to use API Gateway**

* In private projects or small companies where security and central management is not the highest priority.
* If the number of Microservices is fairly small.

**Enabling Technology Examples**

[Amazon API Gateway](https://aws.amazon.com/api-gateway/), [Azure API Management](https://docs.microsoft.com/en-us/azure/api-management/), [Apigee](https://cloud.google.com/apigee), [Kong](https://konghq.com/kong/), [WSO2 API Manager](https://wso2.com/api-management/)

**Further Reading**[Microservices Pattern: API gateway patternLet's imagine you are building an online store that uses the Microservice architecture pattern and that you are…microservices.io](https://microservices.io/patterns/apigateway.html)[API gateways - Azure Architecture CenterIn a microservices architecture, a client might interact with more than one front-end service. Given this fact, how…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/microservices/design/gateway)

### Strangler <a href="#3d99" id="3d99"></a>

If we want to use Microservice Architecture in a brownfield project, we need to migrate legacy or existing Monolithic applications to Microservices. Moving an existing, large, in-production Monolithic applications to Microservices is quite challenging as it may disrupt the application’s availability.

One solution is to use the Strangler pattern. Strangler pattern means incrementally migrating a Monolithic application to Microservice Architecture by gradually replacing specific functionality with new Microservices. Also, new functionalities are only added in Microservices, bypassing the legacy Monolithic application. A Facade (API Gateway) is then configured to route the requests between the legacy Monolith and Microservices. Once the functionality is migrated from the Monolith to Microservices, the Facade then intercepts the client request and route to the new Microservices. Once all the legacy monolith's functionalities are migrated, the legacy Monolithic application is “strangled,” i.e., decommissioned.![](https://miro.medium.com/max/60/1\*4cO7G9QFc9OjQgmSTwQP1Q.jpeg?q=20)![](https://miro.medium.com/max/1151/1\*4cO7G9QFc9OjQgmSTwQP1Q.jpeg)Strangler by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

### Pros <a href="#46cc" id="46cc"></a>

* Safe migration of Monolithic application to Microservices.
* The migration and new functionality development can go in parallel.
* The migration process can have its own pace.

**Cons**

* Sharing Data Store between the existing Monolith and new Microservices becomes challenging.
* Adding a Facade (API Gateway) will increase the system latency.
* End-to-end testing becomes difficult.

**When to use Strangler**

* Incremental migration of a large Backend Monolithic application to Microservices.

**When not to use Strangler**

* If the Backend Monolith is small, then wholesale replacement is a better option.
* If the client request to the legacy Monolithic application cannot be intercepted.

**Enabling Technology**

Backend application frameworks with API Gateway.

**Further Reading**[bliki: StranglerFigApplicationWhen Cindy and I went to Australia, we spent some time in the rain forests on the Queensland coast. One of the natural…martinfowler.com](https://martinfowler.com/bliki/StranglerFigApplication.html)[Strangler pattern - Cloud Design PatternsIncrementally migrate a legacy system by gradually replacing specific pieces of functionality with new applications and…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler)[Microservices Pattern: Strangler applicationHow do you migrate a legacy monolithic application to a microservice architecture? Modernize an application by…microservices.io](https://microservices.io/patterns/refactoring/strangler-application.html)

### Circuit Breaker <a href="#fcee" id="fcee"></a>

In Microservice Architecture, where the Microservices communicates Synchronously, a Microservice usually calls other services to fulfill business requirements. Call to another service can fail due to transient faults (slow network connection, timeouts, or temporal unavailability). In such cases, retrying calls can solve fix the issue. However, if there is a severe issue (complete failure of the Microservice), then the Microservice is unavailable for a longer time. Retrying is pointless and wastes precious resources (thread is blocked, waste of CPU cycles) in such scenarios. Also, the failure of one service might lead to cascading failures throughout the application. In such scenarios, fail immediately is a better approach.

The Circuit Breaker pattern can come to the rescue for such use cases. A Microservice should request another Microservice via a proxy that works similarly to an **Electrical Circuit Breaker.** The proxy should count the number of recent failures that have occurred and use it to decide whether to allow the operation to proceed or simply return an exception immediately.![](https://miro.medium.com/max/60/1\*Olh9J1L3JSDi-PUa9CGa\_A.jpeg?q=20)![](https://miro.medium.com/max/996/1\*Olh9J1L3JSDi-PUa9CGa\_A.jpeg)Circuit Breaker by [Md Kamaruzzaman](https://medium.com/@md.kamaruzzaman)

The Circuit Breaker can have the following three states:

* _Closed:_ The Circuit Breaker routs requests to the Microservice and counts the number of failures in a given period of time. If the number of failures in a certain period of time exceeds a threshold, it then trips and goes to Open State.
* _Open_: Request from the Microservice fails immediately, and an exception is returned. After a timeout, the Circuit Breaker goes to a Half-Open state.
* _Half-Open_: Only a limited number of requests from the Microservice are allowed to pass through and invoke the operation. If these requests are successful, the circuit breaker goes to a Closed state. If any request fails, the Circuit Breaker goes to Open state.

### Pros <a href="#921e" id="921e"></a>

* Improve the fault-tolerance and resilience of the Microservice Architecture.
* Stops the cascading of failure to other Microservices.

**Cons**

* Need sophisticated Exception handling.
* Logging and Monitoring.
* Should support manual reset.

**When to use Circuit Breaker**

* In tightly coupled Microservice Architecture where Microservices communicates Synchronously.
* If one Microservice has a dependency on multiple other Microservices.

**When not to use Circuit Breaker**

* Loosely coupled, event-driven Microservice Architecture.
* If a Microservice has no dependency on other Microservices.

**Enabling Technology**

API Gateway, Service Mesh, various Circuit Breaker Libraries ([Hystrix](https://github.com/Netflix/Hystrix/wiki/How-it-Works), [Reselience4J](https://github.com/resilience4j/resilience4j), [Polly](http://www.thepollyproject.org).

**Further Reading**[bliki: CircuitBreakerIt's common for software systems to make remote calls to software running in different processes, probably on different…martinfowler.com](https://martinfowler.com/bliki/CircuitBreaker.html)[Circuit Breaker pattern - Cloud Design PatternsHandle faults that might take a variable amount of time to recover from, when connecting to a remote service or…docs.microsoft.com](https://docs.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker)[Microservices Pattern: Circuit BreakerYou have applied the Microservice architecture. Services sometimes collaborate when handling requests. When one service…microservices.io](https://microservices.io/patterns/reliability/circuit-breaker.html)

### Externalized Configuration <a href="#9675" id="9675"></a>

Every business Application has many configuration parameters for various Infrastructure (e.g., Database, Network, connected Service addresses, credentials, certificate path). Also, in an enterprise environment, the application is usually deployed in **various runtimes (Local, Dev, Prod)**. One way to achieve this is via the Internal Configuration, which is a fatal bad practice. It can lead to severe security risk as production credentials can easily be compromised. Also, any change in configuration parameter needs to rebuild the Application. This is even more critical in Microservice Architecture as we potentially have hundreds of services.

The better approach is to externalize all the Configurations. As a result, the build process is separated from the runtime environment. Also, it minimizes the security risk as the Production configuration file is only used during runtime or via environment variables.

### Pros <a href="#4ddd" id="4ddd"></a>

* Production configurations are not part of the Codebase and thus minimize security vulnerability.
* Configuration parameters can be changed without a new build.

**Cons**

* We need to choose a framework that supports the Externalized Configuration.

**When to use Externalized Configuration**

* Any serious production application must use Externalized Configuration.

**When not to use Externalized Configuration**

* In proof of concept development.

**Enabling Technology**

Almost all enterprise-grade, modern frameworks support Externalized Configuration.

**Further Reading**[Microservices Pattern: Externalized configurationAn application typically uses one or more infrastructure and 3rd party services. Examples of infrastructure services…microservices.io](https://microservices.io/patterns/externalized-configuration.html)[Build Once, Run Anywhere: Externalize Your ConfigurationMost software that does more than a "hello world" needs to be configured in some way or another in order to function in…reflectoring.io](https://reflectoring.io/externalize-configuration/)

### Consumer-Driven Contract Testing <a href="#f4f7" id="f4f7"></a>

In Microservice Architecture, there are many Microservices often developed by separate teams. These microservices work together to fulfill a business requirement (e.g., customer request) and communicate with each other Synchronously or Asynchronously. Integration testing of a **Consumer** Microservice is challenging. Usually, **TestDouble** is used in such scenarios for a faster and cheaper test run. But TestDouble **** often **** does not represent the real **Provider** Microservice. Also, if the Provider Microservice changes its API or Message, then TestDouble **** fails to acknowledge that. The other option is to make end-to-end testing. While end-to-end testing is mandatory before production, it is brittle, slow, expensive, and is no replacement for Integration testing ([Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)).

Consumer-Driven contract testing can help us in this regard. Here, the Consumer Microservice owner team write a test suite containing its Request and expected Response (for Synchronous communication) or expected messages (for Asynchronous communication) for a particular Provider Microservice. These test suites are called explicit **Contracts**. For a Provider Microservice, all the Contract test suites of its Consumers are added in its automated test. When the automated test for a particular Provider Microservice is performed, it runs its own tests and the Contracts and verifies the Contract. In such a way, the contract test can help maintain the integrity of the Microservice Communication in an automated way.

### Pros <a href="#94a2" id="94a2"></a>

* If the Provider changes the API or Message unexpectedly, it is found autonomously in a short time.
* Less surprise and more robustness, especially an enterprise application containing lots of Microservices.
* Improved team autonomy.

**Cons**

* Need extra work to develop and integrate Contract tests in Provider Microservice as they may use completely different test tools.
* If the Contract test does not match real Service consumption, it may lead to production failure.

**When to use Consumer-Driven Contract Testing**

* In large-scale enterprise business applications, where typically, different teams develop different services.

**When not to use Consumer-Driven Contract Testing**

* Relative simpler, smaller applications where one team develops all the Microservices.
* If the Provider Microservices are relatively stable and not under active development.

**Enabling Technology**

[Pact](https://docs.pact.io), [Postman](https://www.postman.com), [Spring Cloud Contract](https://spring.io/guides/gs/contract-rest/)

**Further Reading**[Consumer-Driven Contracts: A Service Evolution PatternIan Robinson To illustrate some of the problems we encounter while evolving services, consider a simple ProductSearch…martinfowler.com](https://martinfowler.com/articles/consumerDrivenContracts.html)[Microservices Pattern: Service Integration Contract TestYou have applied the Microservice architecture pattern. The application consists of numerous services. Services often…microservices.io](https://microservices.io/patterns/testing/service-integration-contract-test.html)[What is consumer driven contract testing?Consumer driven contract testing is a type of contract testing \[/what-is-contract-testing-page\] that ensures that a…pactflow.io](https://pactflow.io/what-is-consumer-driven-contract-testing/)

## Conclusion <a href="#8f21" id="8f21"></a>

In the modern large-scale enterprise Software development, Microservice Architecture can help development scaling with many long-term benefits. But Microservice Architecture is no Silver Bullet that can be used in every use case. If it is used in the wrong type of application, Microservice Architecture can give more pains as gains. The development team that wants to adopt Microservice Architecture should follow a set of best practices and use a set of reusable, battle-hardened design patterns.

The most vital design pattern in Microservice Architecture is the **Database per Microservice**. Implementing this design pattern is challenging and needs several other closely related design patterns (**Event Sourcing, CQRS, Saga**). In typical business applications with multiple Clients (Web, Mobile, Desktop, Smart Devices), the communications between Client and Microservices can be chatty and may require Central control with added Security. The design patterns **Backends for Frontends** and **API Gateway** are very useful in such scenarios. Also, the **Circuit Breaker** pattern can greatly help to handle error scenarios in such applications. Migrating legacy Monolithic application to Microservices is quite challenging, and the **Strangler** pattern can help the migration. The **Consumer-Driven Contract Test** is an instrumental pattern for the Integration Testing of Microservices. At the same time, **Externalize Configuration** is a mandatory pattern in any modern application development.

This list is not all-inclusive, and depending on your use case, you may need other design patterns. But this list will give you an excellent introduction to Microservice Architecture Design Patterns.
