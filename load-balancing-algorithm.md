# Load Balancing Algorithm

##

Load balancing is the process of distributing network traffic across multiple servers. This ensures no single server bears too much workload. By spreading the work evenly, load balancing improves application responsiveness. It also increases availability of applications and websites for users. Modern applications cannot run without load balancers.

Load balancers manage the flow of information between the server and an endpoint device (PC, laptop, tablet or smartphone). The server could be on-premises, in a data center or the public cloud. The server can also be physical or virtualized. The load balancer helps servers move data efficiently and prevents server overloads. Load balancers conduct continuous health checks on servers to ensure they can handle requests. If necessary, the load balancer can also remove unhealthy servers from the pool until they are restored. Some load balancers even trigger the creation of new virtualized application servers to cope with increased demand.

### Hardware vs. Software Load Balancers <a href="#4b14" id="4b14"></a>

Load balancers typically come in two flavors : hardware‑based and software‑based. Vendors of hardware‑based solutions load proprietary software onto the machine they provide, which often uses specialized processors. To cope with increasing traffic at your website, you have to buy more or bigger machines from the vendor. Software solutions generally run on commodity hardware, making them less expensive and more flexible. You can install the software on the hardware of your choice or in cloud environments like AWS EC2.

### Software Load Balancers <a href="#28c5" id="28c5"></a>

Advantages :

* Flexibility to adjust for changing needs.
* Ability to scale beyond initial capacity by adding more software instances.
* Lower cost than purchasing and maintaining physical machines. Software can run on any standard device, which tends to be cheaper.
* Allows for load balancing in the cloud, which provides a managed, off-site solution that can draw resources from an elastic network of servers. Cloud computing also allows for the flexibility of hybrid hosted and in-house solutions. The main load balancer could be in-house while the backup is a cloud load balancer.

Disadvantages :

* When scaling beyond initial capacity, there can be some delay while configuring load balancer software.
* Ongoing costs for upgrades.

### Hardware Load Balancers <a href="#19be" id="19be"></a>

Advantages :

* Fast throughput due to software running on specialized processors.
* Increased security since only the organization can access the servers physically.
* Fixed cost once purchased.

Disadvantages :

* Require more staff and expertise to configure and program the physical machines.
* Inability to scale when the set limit on number of connections has been made. Connections are refused or service degraded until additional machines are purchased and installed.
* Higher cost for purchase and maintenance of physical network load balancer. Owning a hardware load balancer may also require paying for consultants to manage it.

### Dynamic load balancing algorithms <a href="#26b7" id="26b7"></a>

* _Least connection:_ Checks which servers have the fewest connections open at the time and sends traffic to those servers. This assumes all connections require roughly equal processing power.
* _Weighted least connection:_ Gives administrators the ability to assign different weights to each server, assuming that some servers can handle more connections than others.
* _Weighted response time:_ Averages the response time of each server, and combines that with the number of connections each server has open to determine where to send traffic. By sending traffic to the servers with the quickest response time, the algorithm ensures faster service for users.
* _Resource-based:_ Distributes load based on what resources each server has available at the time. Specialized software (called an “agent”) running on each server measures that server’s available CPU and memory, and the load balancer queries the agent before distributing traffic to that server.

### Static load balancing algorithms <a href="#783b" id="783b"></a>

* _Round robin:_ Round robin load balancing distributes traffic to a list of servers in rotation using the Domain Name System (DNS). An authoritative nameserver will have a list of different A records for a domain and provides a different one in response to each DNS query.
* _Weighted round robin:_ Allows an administrator to assign different weights to each server. Servers deemed able to handle more traffic will receive slightly more. Weighting can be configured within DNS records.
* _IP hash:_ Combines incoming traffic’s source and destination IP addresses and uses a mathematical function to convert it into a hash. Based on the hash, the connection is assigned to a specific server.

## Load Balancer&#x20;

#### L4 Load Balancer

It operates on the transport layer. The load balancer’s IP is advertised to clients. Clients send packets to the load balancer. The load balancer has internal forwarding rules to decide where to route the packets. It’s often implemented via network address translation (NAT), which uses the combination of IP address and port to resolve the one-load-balancer-to-many-application-servers relationship. A L4 load balancer typically only needs to inspect and modify a few packet headers — such as source/destination IP and port — and does not need to care about the content inside the packets.

#### L7 Load Balancer

t operates on the application layer and it’s sometimes regarded as a synonym to HTTP(s) load balancers. Clients send requests to the load balancer. The load balancer inspects the request content and determines the best application server to forward the requests. L7 load balancers are becoming more popular as they can make smarter decisions than L4 load balancers though they’re more resource-intensive.

There are other types of load balancing techniques that don’t rely on extra boxes in a system architecture diagram. For example, DNS could be configured to map a domain to multiple IP addresses and return those IP addresses in response to DNS requests. This to some extent enables client-side load balancing as clients now get to choose which IP it prefers. Clients can also just randomly choose one. Another example would be IP anycast. It could be set up so that multiple endpoints “share” the same IP address. The network then routes traffic to the appropriate endpoints depending on proximity and other factors. This is commonly used in global content delivery networks

#### Routing Algorithms

There are many routing algorithms. In fact, it’s a deep research area. But in the context of a system design interview, the smartness of the routing depends on the information the load balancer processes. It can do a blind round-robin. It can hash the source IP. It can choose an application server based on liveliness, resource, response time, bandwidth, and number of open connections. L7 load balancers which inspect the request content can determine the best forwarding based on request URL, for example, serving static contents from a set of application servers and transactional processing from another.

If possible, a desired behavior of the load balancers is to route traffic from the same source or within the same session to the same application server. It enables efficient server-side caching and persists the client sessions, which are often critical in some business logic.

&#x20;**SSL & HTTPs Offload/Termination**

Sophisticate load balancers can act as the other end (w.r.t clients) of the secure connection and decrypt the request contents. If you trust your internal network, unwrapping the content from the encrypted packets/requests are often desirable because those security protocols add latency. Another reason is that you may want to limit the places where you need to manage security and certifications to reduce administrative overhead and attack surface. Even if you really want to encrypt the traffic inside your internal network to provide defense in depth, you may still want to decrypt it at its entry point to inspect the contents and stop bad requests at the gate.

It’s worth clarifying that L4 and below load balancers don’t offload/terminate SSL or HTTPs because they don’t even look at the packet content.

#### **Single Point of Failure**

The canonical way to address single point of failure in load balancing is indeed through redundancy. But there is no magic. Multiple instances of the load balancers are created in the network. Their IPs are all mapped to the same domain in DNS. Those IPs are returned to clients by DNS. Clients can choose any one to connect. The DNS responds with a short TTL for that domain so that clients are forced to consult DNS regularly to get the up-to-date load balancer IP addresses in case any of them fails. This one-domain-to-multiple-IPs is in itself a form of load balancing that we discussed earlier. It’s leveraged by load balancers so that clients can at least get to one load balancer where subsequent and more sophisticated routings can be invoked.





* &#x20;[Deterministic Aperture: A distributed, load balancing algorithm](https://blog.twitter.com/engineering/en\_us/topics/infrastructure/2019/daperture-load-balancer.html)
*
