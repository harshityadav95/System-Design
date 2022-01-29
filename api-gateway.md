# API Gateway

API gateway is a typical pattern that many API developers are using to encapsulate their API endpoints. But, it is common that many are using API gateway anti-patterns as well. This article focuses on the correct usage of the API Gateway pattern.

## What is an API Gateway ? <a href="#79e3" id="79e3"></a>

An API(Application Programming Interface) Gateway is an interface where it sits in front of other back-end (Micro)services. API Gateway is the single-entry point for the back-end architecture where the communication channel normally ends in a database. API Gateway is ensuring architecture characteristics such as security, scalability, and high availability.

Can we implement our services (e.g., RESTFull or SOAP ) without using an API Gateway? Of course, we can. But the individual services need many non-business-specific functionalities such as Routing, authentication and authorization, TLS, Logging, and tracing, etc… Most importantly, if you do not use an API Gateway to expose your API endpoints, a client would know the back-end service distributions and deployment details. This is not a big issue if you are using your API endpoints to integrate with an internal front-end application. This is not a good practice for an Enterprise API Gateway, which can be exposed to unknown, on-demand clients.![](https://miro.medium.com/max/60/1\*Y6MAKrjuPy3AjejQJUq6jg.png?q=20)![](https://miro.medium.com/max/700/1\*Y6MAKrjuPy3AjejQJUq6jg.png)Diagram 1 : API Gateway

## Can we use a Reverse Proxy or a Load balancer as a Gateway? <a href="#9840" id="9840"></a>

If you are using your Gateway only to abstract a single back-end or just to terminate your TLS communication, Reverse proxy or Load balance would be sufficient. But, inherently, a proper API Gateway would fulfill the following functionalities.

* Multiple Back-ends ( Microservices )
* Service Discovery
* Circuit breaking
* Authentication and Authorization
* Rate limiting
* Logging and tracing
* Routing
* Retry logic etc..

## Single API Gateway, Good or Bad? <a href="#77f4" id="77f4"></a>

If you deploy a single API Gateway, even though it is simple and easy to manage, you are asking for the following troubles.

**Single point of failure:** If your API Gateway stops functioning properly, your entire back-end communication will go down. This is a disastrous situation and should be avoided at all costs. As a solution, you can have a scalable API Gateway cluster behind a hardware load balancer where the load balancer can be notified of the backpressure.

**Various traffic requirements would traverse through a single API Gateway stack:** Depend on the various business functionalities, the API requirements are different in terms of performance, security, compliances, etc. If you do have a single API Gateway cluster, all traffic will traverse through that, and you won’t be able to cater to different needs as required. This is not a good idea, especially if you are building an Enterprise API Gateway.

**Make your DevOps functionality difficult:** If you do any routing change or protocol change in any of your back-end services, it will impact your API Gateway as well. Hence, once you deploy such a change, the API gateway also needs to get updated using a single cluster of API Gateway.

Hence, the best option would be to have multiple API Gateways, specifically designed to cater to specific needs.![](https://miro.medium.com/max/60/1\*dIye\_6PekdMMOV9uDtTTXw.png?q=20)![](https://miro.medium.com/max/700/1\*dIye\_6PekdMMOV9uDtTTXw.png)Diagram 2 : More accurate usage of API Gateway

### We can identify two flavors of API Gateways as follows. <a href="#0253" id="0253"></a>

* Enterprise API Gateway
* Microservices API Gateway

The Enterprise API Gateways are mainly using for the API marketplace where you are expecting any 3rd party to come and use your API after paying for the usage. Most of the API integrations cater to this nature, and the following are the main concerns you need to focus on when developing an Enterprise API Gateway.

* A separate enterprise integration team would do deployment. A manual process of QA →Staging → UAT → Production would be used.
* DevOps pipelines would hardly be used in this kind of scenario.
* Error encapsulation(Custom errors) for clients.
* Need to monitor invocation rates and HTTPS status. Maybe your billing would depend on this.
* We need to focus more on security, data compliance, and standards, etc.

In Microservices, Gateway’s concerns are as follows.

* Using inside your ecosystem where the API Gateway is exposed only to your internal clients.
* High usage of CI/CD pipelines with deployments types such as Canary, Shadow, Blue-Green, etc.
* The API may expose full details of errors along with the stack trace.
* Non-functional requirements such as latency and performance would be considered over enterprise-level constraints such as standards, compliance, security, etc.

## API Gateway Anti-patterns <a href="#0f98" id="0f98"></a>

Following can be identified as the API Gateway Anti-patterns.

* Overburning the Gateway by routing most of the traffic through a single service.
* Not considering the scalability and resiliency factors of services that API Gateway is dependent upon ( Eg: Authentication, Service discovery. Routing, etc…) Failure of these services would affect directly to the API Gateway’s availability.
* Avoid using the API Gateway as ESB by rerouting internal traffic using the API Gateway. The API Gateway should route only the external incoming traffic to your service layer.
* Ensure a single point of failure is mitigated and managed.
* Let your API Gateways unmonitored and isolated.

What are the commercial and open source products that we can use.

[Kong Gateway](https://konghq.com/kong/)\
[Apache APISIX](https://apisix.apache.org)\
[Tyk](https://tyk.io/features/api-gateway/)\
[Goku](https://github.com/ThreeMammals/Ocelot)\
[WSO2](https://wso2.com/api-management/)\
[KrakenD](https://www.krakend.io)\
[Zuul](https://www.google.com/url?sa=t\&rct=j\&q=\&esrc=s\&source=web\&cd=\&cad=rja\&uact=8\&ved=2ahUKEwiwrO2OzYzwAhVPgUsFHTj9D5kQFjAAegQIBhAD\&url=https%3A%2F%2Fspring.io%2Fguides%2Fgs%2Frouting-and-filtering%2F\&usg=AOvVaw0wIVR1x3RVigHszRR8CZom)\


## Microservices Design - API Gateway Pattern <a href="#bb04" id="bb04"></a>

[![Bibek Shah](https://miro.medium.com/fit/c/96/96/2\*duBnxiJEQgjdUOGFen3ooQ.png)](https://medium.com/@bibekshah09?source=post\_page-----980e8d02bdd5--------------------------------)[Bibek Shah](https://medium.com/@bibekshah09?source=post\_page-----980e8d02bdd5--------------------------------)[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F\_%2Fsubscribe%2Fuser%2Fe10d35ca44f2\&operation=register\&redirect=https%3A%2F%2Fblog.devgenius.io%2Fmicroservices-design-api-gateway-pattern-980e8d02bdd5\&user=Bibek%20Shah\&userId=e10d35ca44f2\&source=post\_page-e10d35ca44f2----980e8d02bdd5---------------------follow\_byline-----------)[Jul 4, 2020](https://blog.devgenius.io/microservices-design-api-gateway-pattern-980e8d02bdd5?source=post\_page-----980e8d02bdd5--------------------------------) · 7 min read![](https://miro.medium.com/max/1400/1\*IlIZ5M7-ij-FcVtax0eVlQ.jpeg)Gateway to Ghorepani Poonhill, Nepal (source: [Wikipedia](https://www.wikipedia.org))

According to the definition by Gartner: _**“Microservice is a tightly scoped, strongly encapsulated, loosely coupled, independently deployable, and independently scalable application component**.”_

The goal of the microservices is to sufficiently decompose/decouple the application into loosely coupled microservices/modules in contrast to monolithic applications where modules are highly coupled and deployed as a single big chunk. This will be helpful due to the following reasons:

* Each microservice can be deployed, upgraded, scaled, maintained, and restarted independent of sibling services in the application.
* Agile development & agile deployment with an autonomous cross-functional team.
* Flexibility in using technologies and scalability.

Different loosely coupled services are deployed based upon their own specific needs where each service has its fine-grained APIs model to serve different clients (Web, Mobile, and 3rd party APIs).

### Client to Microservices connections <a href="#ca17" id="ca17"></a>

![](https://miro.medium.com/max/60/1\*qjKOOT85ESeznHyZCd\_UWA.png?q=20)![](https://miro.medium.com/max/700/1\*qjKOOT85ESeznHyZCd\_UWA.png)Fig. Communication in Microservices

While thinking of the client **directly communicating** with each of the deployed microservices, the following challenges should be taken into consideration:

1. In the case where microservice is exposing fine-grained APIs to the client, the client should request to each microservice. In a typical single page, it may be required for **multiple server round trips** in order to fulfill the request. This may be even worse for low network operating devices such as mobile.
2. **Diverse communication protocol (**such as gRpc, thrift, REST, AMQP e.t.c**)** existing in the microservices makes it challenging and bulky for the client to adopt all those protocols.
3. **Common gateway functionalities** (such as authentication, authorization, logging) have to be implemented in each microservice.
4. It will be **difficult to make changes in microservices** without disrupting client connection. For e.g while merging or dividing microservices, it may be required to recode the client section.

## API Gateway <a href="#f4a2" id="f4a2"></a>

To address the above-mentioned challenges, an additional layer is introduced that sits between the client and the server acting as a reverse proxy routing request from the client to the server. Similar to the facade pattern of Object-Oriented Design, it provides a single entry point to the APIs encapsulating the underlying system architecture which is called API Gateway.

In short, it behaves precisely as API management but it is important not to confuse [API management with API Gateway](https://www.getambassador.io/docs/latest/topics/concepts/microservices-api-gateways/).![](https://miro.medium.com/max/60/1\*0vZX\_beYLJJt7TQ4jYoDEg.png?q=20)![](https://miro.medium.com/max/700/1\*0vZX\_beYLJJt7TQ4jYoDEg.png)Fig. Microservice API Gateway

## Functionalities of API Gateway: <a href="#e2c1" id="e2c1"></a>

### Routing <a href="#2cdc" id="2cdc"></a>

Encapsulating the underlying system and decoupling from the clients, the gateway provides a single entry point for the client to communicate with the microservice system.

### Offloading <a href="#b8df" id="b8df"></a>

API gateway consolidates the edge functionalities rather than making every microservices implementing them. Some of the functionalities are:

* Authentication and authorization
* Service discovery integration
* Response caching
* Retry policies, circuit breaker, and QoS
* Rate limiting and throttling
* Load balancing
* Logging, tracing, correlation
* Headers, query strings, and claims transformation
* IP whitelisting
* IAM
* Centralized Logging (transaction ID across the servers, error logging)
* Identity Provider, Authentication and Authorization

## Backend for Frontend (BFF) pattern <a href="#ecff" id="ecff"></a>

It is a **variation of the API Gateway pattern**. Rather than a single point of entry for the clients, it provides multiple gateways based upon the client. The purpose is to provide tailored APIs according to the needs of the client, removing a lot of bloats caused by making generic APIs for all the clients.![](https://miro.medium.com/max/60/1\*JTjhbxRxevEfnt4eExWTVw.png?q=20)![](https://miro.medium.com/max/700/1\*JTjhbxRxevEfnt4eExWTVw.png)Fig. Backend For Frontend (BFF) Pattern

### How many BFFs do you need? <a href="#0f54" id="0f54"></a>

The base concept of BFF is developing niche backends for each user experience. The guideline by [Phil Calçado](https://philcalcado.com) is ‘**one experience, one BFF**’. If the requirements across clients (IOS client, android client, a web browser e.t.c) vary significantly and the time to market of a single proxy or API becomes problematic, BFFs are a good solution. It should also be noted that the more complex design requires a complex setup.

## GraphQL and BFF <a href="#9ecf" id="9ecf"></a>

[GraphQL](https://graphql.org) is a query language for your API. [Phil Calçado](https://philcalcado.com) presents in this [article](https://philcalcado.com/2019/07/12/some\_thoughts\_graphql\_bff.html) that BFF and GraphQL are related but not mutually exclusive concepts. He adds that BFFs are not about the shape of your endpoints, but about giving your client applications autonomy where you can build your GraphQL APIs as many BFFs or as an OSFA (one-size-fits-all) API.

## Notable API Gateways <a href="#cc96" id="cc96"></a>

### Netflix API Gateway: [Zuul](https://github.com/Netflix/zuul) <a href="#47bf" id="47bf"></a>

The [Netflix](https://netflixtechblog.com/embracing-the-differences-inside-the-netflix-api-redesign-15fd8b3dc49d) streaming service available on more than 1000 different device types (televisions, set‑top boxes, smartphones, gaming systems, tablets, e.t.c) handing over 50,000 requests per second during peak hours, found substantial limitations in OSFA (one-size-fits-all) REST API approach and used the API Gateway tailored for each device.

Zuul 2 at Netflix is the front door for all requests coming into Netflix’s cloud infrastructure. Zuul 2 significantly improves the architecture and features that allow our gateway to handle, route, and protect Netflix’s cloud systems, and helps provide our 125 million members the best experience possible.![](https://miro.medium.com/max/60/1\*9IBt7heCiWdHl5o78VeFVA.png?q=20)![](https://miro.medium.com/max/700/1\*9IBt7heCiWdHl5o78VeFVA.png)Fig. Zuul in Netflix Cloud Architecture (Image Source: [https://netflixtechblog.com](https://netflixtechblog.com))

### Amazon API Gateway <a href="#9a96" id="9a96"></a>

AWS provides fully managed service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket where developers can create APIs that access AWS or other web services, as well as data stored in the [AWS Cloud](https://aws.amazon.com/what-is-cloud-computing/).![](https://miro.medium.com/max/60/0\*ANufset6xld70D1W?q=20)![](https://miro.medium.com/max/700/0\*ANufset6xld70D1W)Fig. AWS API Gateway

### Kong API Gateway <a href="#ae03" id="ae03"></a>

Kong Gateway is an open-source, lightweight API gateway optimized for microservices, delivering unparalleled latency performance and scalability. If you just want the basics, this option will work for you. It is scalable easily horizontally by adding more nodes. It supports large and variable workloads with very low latency.![](https://miro.medium.com/max/60/0\*ZPmJJWsxTsrP3-Yf?q=20)![](https://miro.medium.com/max/700/0\*ZPmJJWsxTsrP3-Yf)Fig. Kong API Gateway

### Other API Gateways <a href="#fa42" id="fa42"></a>

* [Apigee API Gateway](https://apigee.com/api-management/)
* [MuleSoft](https://www.mulesoft.com/platform/api-management)
* [Tyk.io](https://github.com/TykTechnologies/tyk)
* [Akana](https://www.akana.com/products/api-platform/api-gateway)
* [SwaggerHub](https://swagger.io/tools/swaggerhub/)
* [Azure API Gateway](https://azure.microsoft.com/en-us/services/api-management/)
* [Express API Gateway](https://www.express-gateway.io)
* [Karken D](https://www.krakend.io)

### Choosing the right API gateway <a href="#c2a6" id="c2a6"></a>

Some of the common baseline for evaluation criteria include simplicity, open-source vs propriety, scalability & flexibility, security, features, community, administrative (support, monitoring & deployment), environment provisioning(installation, configuration, hosting offering), pricing, and documentation.

## API Composition / Aggregation <a href="#bc64" id="bc64"></a>

Some API requests in API Gateway map directly to single service API which can be served by routing request to the corresponding microservice. However, in the case of complex API operations that requires results from several microservices can be served by **API composition/aggregation** (a **** scatter-gather mechanism). In case of dependency of one another service where synchronous communication is required, the chained composition pattern has to be followed. The composition layer has to support a significant portion of ESB/integration capabilities such as transformations, orchestration, resiliency, and stability patterns.

A root container is deployed with the special distributor and aggregator functionalities (or microservices). The distributor is responsible for breaking down into granular tasks and distributing those tasks to microservice instances. The aggregator is responsible for aggregating the results derived by business workflow from composed microservice.

### API Gateway and Aggregation <a href="#80c1" id="80c1"></a>

API gateway with added features results in overambitious gateways that encourage designs that continue to be difficult to test and deploy. It is highly recommended to avoid aggregation and data transformation in the API Gateway. Domain smarts are better suited to be done in application code that follows the defined software development practices. Netflix API Gateway, [Zuul 2](https://github.com/netflix/zuul/) removed a lot of the business logic from Gateway that they had in Zuul to origin systems. For more details, refer [here](https://www.infoq.com/news/2016/10/netflix-zuul-asynch-nonblocking/).![](https://miro.medium.com/max/60/1\*9ktQNGUHWX6XaiVM3kHDyA.png?q=20)![](https://miro.medium.com/max/700/1\*9ktQNGUHWX6XaiVM3kHDyA.png)Fig. Composite / Integration service in layered Microservices

## Service Mesh and API Gateway <a href="#ae78" id="ae78"></a>

Service mesh in microservices is a configurable network infrastructure layer that handles interprocess communication. This is akin to what is often termed as sidecar proxy or sidecar gateway. It provides a lot of functionalities such as:

* Load Balancing
* Service Discovery
* Health Checks
* Security

On the surface, **it appears as though API gateways and service meshes solve the same problem and are therefore redundant. They do solve the same problem but in different contexts.** API gateway is deployed as a part of a business solution that is discoverable by the external clients handling north-south traffic(face external client), however, service mesh handles east-west traffic (among different microservices).

Implementing service mesh avoids the resilient communication pattern such as circuit breakers, discovery, health checks, service observability in your own code. For a small number of microservices, alternative strategies for failure management should be considered as service mesh integration may overkill you. For a larger number of microservices, it will be beneficial.

Combining these two technologies can be a powerful way to ensure application uptime and resiliency while ensuring your applications are easily consumable. Viewing two as a contemporary can be a bad idea and it is better to view two as being complementary to one another in deployments that involve both microservices and APIs.![](https://miro.medium.com/max/60/1\*t1\_sflyFisbrDYsbzSJq\_g.png?q=20)![](https://miro.medium.com/max/700/1\*t1\_sflyFisbrDYsbzSJq\_g.png)Fig Layered Microservices with Service Mesh

### Considerations for API Gateway implementation: <a href="#1783" id="1783"></a>

* Possible single point of failure or bottleneck.
* Increase in response time due to additional network hop through API Gateway and risk of complexity.

### References: <a href="#9ef8" id="9ef8"></a>

1. [https://microservices.io/index.html](https://microservices.io/index.html)
2. [https://docs.microsoft.com/en-us/azure/architecture/](https://docs.microsoft.com/en-us/azure/architecture/)
3. [https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md](https://github.com/wso2/reference-architecture/blob/master/api-driven-microservice-architecture.md)
4. [https://tsh.io/blog/design-patterns-in-microservices-api-gateway-bff-and-more/](https://tsh.io/blog/design-patterns-in-microservices-api-gateway-bff-and-more/)
5. [https://www.infoq.com/articles/service-mesh-ultimate-guide/](https://www.infoq.com/articles/service-mesh-ultimate-guide/)
6. [https://samnewman.io/patterns/architectural/bff/](https://samnewman.io/patterns/architectural/bff/)
7. [https://netflixtechblog.com/](https://netflixtechblog.com)
