# Load Balancing Algorithm

## Load Balancer 

#### L4 Load Balancer

It operates on the transport layer. The load balancer’s IP is advertised to clients. Clients send packets to the load balancer. The load balancer has internal forwarding rules to decide where to route the packets. It’s often implemented via network address translation \(NAT\), which uses the combination of IP address and port to resolve the one-load-balancer-to-many-application-servers relationship. A L4 load balancer typically only needs to inspect and modify a few packet headers — such as source/destination IP and port — and does not need to care about the content inside the packets.

#### L7 Load Balancer

t operates on the application layer and it’s sometimes regarded as a synonym to HTTP\(s\) load balancers. Clients send requests to the load balancer. The load balancer inspects the request content and determines the best application server to forward the requests. L7 load balancers are becoming more popular as they can make smarter decisions than L4 load balancers though they’re more resource-intensive.

There are other types of load balancing techniques that don’t rely on extra boxes in a system architecture diagram. For example, DNS could be configured to map a domain to multiple IP addresses and return those IP addresses in response to DNS requests. This to some extent enables client-side load balancing as clients now get to choose which IP it prefers. Clients can also just randomly choose one. Another example would be IP anycast. It could be set up so that multiple endpoints “share” the same IP address. The network then routes traffic to the appropriate endpoints depending on proximity and other factors. This is commonly used in global content delivery networks

#### Routing Algorithms

There are many routing algorithms. In fact, it’s a deep research area. But in the context of a system design interview, the smartness of the routing depends on the information the load balancer processes. It can do a blind round-robin. It can hash the source IP. It can choose an application server based on liveliness, resource, response time, bandwidth, and number of open connections. L7 load balancers which inspect the request content can determine the best forwarding based on request URL, for example, serving static contents from a set of application servers and transactional processing from another.

If possible, a desired behavior of the load balancers is to route traffic from the same source or within the same session to the same application server. It enables efficient server-side caching and persists the client sessions, which are often critical in some business logic.

 **SSL & HTTPs Offload/Termination**

Sophisticate load balancers can act as the other end \(w.r.t clients\) of the secure connection and decrypt the request contents. If you trust your internal network, unwrapping the content from the encrypted packets/requests are often desirable because those security protocols add latency. Another reason is that you may want to limit the places where you need to manage security and certifications to reduce administrative overhead and attack surface. Even if you really want to encrypt the traffic inside your internal network to provide defense in depth, you may still want to decrypt it at its entry point to inspect the contents and stop bad requests at the gate.

It’s worth clarifying that L4 and below load balancers don’t offload/terminate SSL or HTTPs because they don’t even look at the packet content.

#### **Single Point of Failure**

The canonical way to address single point of failure in load balancing is indeed through redundancy. But there is no magic. Multiple instances of the load balancers are created in the network. Their IPs are all mapped to the same domain in DNS. Those IPs are returned to clients by DNS. Clients can choose any one to connect. The DNS responds with a short TTL for that domain so that clients are forced to consult DNS regularly to get the up-to-date load balancer IP addresses in case any of them fails. This one-domain-to-multiple-IPs is in itself a form of load balancing that we discussed earlier. It’s leveraged by load balancers so that clients can at least get to one load balancer where subsequent and more sophisticated routings can be invoked.





*  [Deterministic Aperture: A distributed, load balancing algorithm](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/daperture-load-balancer.html)
* 
