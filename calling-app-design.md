# Calling App Design



The features are listed below:

1. Users can call each other over VOIP or PSTN
2. Call Routing
3. Charge user for making calls
4. Choose a provider for each call

We design the calling app. This video highlights a good attempt at the design, typical of someone with less than 4-5 years of experience in the industry.

Things to notice: The candidate does a very good job at describing what the system will do. However, they do not talk about the deep problems, since they haven't spent time to expand on the requirements.



1.  How much bandwidth do you need to handle calls simultaneously?

    Number of users of the system: 10 million.

    Assume 25% users make a phone call everyday = 2.5 million calls.

    Assume each call to last 1 minute on average. That's 2.5 million minutes.

    Number of minutes in a day = 24 _60 \~ 25_ 60 = 100 \* 15 = 1500.

    Number of calls at any point in time = total\_talk\_time / minutes\_in\_a\_day

    \= 2.5M/1500

    \= 2500 calls.

    For PSTN calls, we need dedicated channels.

    Each call will take about 64kbps. (Modems have 56 kbps)

    Bandwidth has to be at least 64 kbps _2500 calls = 64_ 2.5 Mbps = 160 Mbps.

    During peak hours, the load will be significantly higher. Assume 160 \* 3 \~ 500 Mbps.

    The bandwidth rate of PSTN is quite high. This will make for the bulk of the cost.

    For VOIP calls, we can reuse channels.

    Each concurrent call will take about 100kbps.

    Bandwidth has to be at least 100 kbps \* 2500 calls = 250 Mbps

    During peak hours, the load will be significantly higher. To reduce network congestion, you

    may want to bump up the bandwidth.

    Assume 250 \* 4 \~ 1 Gbps.

    Despite this being twice the requirement of the PSTN bandwidth, the VOIP lines are

    cheaper.

    Total bandwidth requirement will be a mix of VOIP and PSTN. Depending on the user

    requirements, you can purchase the respective bandwidth.
2.  How much RAM memory do you need for caching call states? What if you there is a

    national emergency which triggers a huge calling spike?

    We mentioned having 2500 calls at any given point in time. Each call can be defined by it's

    ID, and some parameters like caller, callee, current state, start time, etc…

    Let's assume we need to store 20 fields per call. Each field is a string of 20 characters.

    \~400 characters per call.

    2500 calls \* 400 characters per call = 10^6 characters = 1 MB

    During a crisis, call volume can shoot up to 20-50x of the normal volume. In terms of RAM

    memory, that's just 50 MB. The requirement here is negligible.
3.  Can you guess which part of the system will be most affected if call volumes shoot up?

    A dramatic call volume increase most likely to affect all bottlenecks of the system. The call

    state manager will need to scale up, but that seems manageable. The bigger issue is the

    switch service.

    The switch has to dynamically scale up to the huge spikey demand. We must have rate

    limiting and scaling measures in place for this scenario. Dynamically acquiring PSTN lines

    will be hard. This is the part that is most likely to fail.
4.  What is the total amount of storage required for the entire system?

    This is a hard estimate, since the internal services may be complex and we have not

    accounted for services such as log aggregators, rate limiters, analytics, etc…

    The call state manager will need state transitions of each call recorded. Permanent.

    The invoice manager will have the summary of each call. Permanent.

    The switch will have detailed logs of state changes. Temporary.

    We may need to record calls from suspicious parties for regulatory/compliance purposes.

    The routing service will need service-provider data and call states to make decisions.

    It's best to clarify what the interview wants. If they insist on the total storage, elaborate on

    each system and sum the components.
5.  What is the total storage required to record calls?

    2.5 million calls are made everyday. Of these, about 0.01% will be suspicious enough to

    track.

    Hence we have 2.5M \* 10^(-4) = 250 calls to be intercepted.

    Each call lasts a minute on average = 250 minutes of storage a day.

    Assume a 1 minute wav file takes 10 MB storage.

    Total storage to record calls per day = 10 MB per minute \* 250 minutes = 2.5 GB everyday

    If we assume 3 copies for fault tolerance and a life of 10 years for a recorded call, we get

    2.5 GB _3_ 365 \* 10

    \~ 2.5 GB _1000_ 10

    \~30 TB

    The total storage requirement for recorded calls is 30 TB.

![](<.gitbook/assets/image (28).png>)

```
Session Service:
Session createSession(user_id, connection_id)
List<Session> getSessions(user_id)
void logoutSession(session_id)

Call State Manager:
Call initiateCall(caller_number, callee_number)
Validations and checks here. Find their profile. Find a route. Check for balance.
Boolean extendLease(Call)

Switch:
Call initiateCall(StateMachine callStateMachine)
StateMachine is an object, where different state transitions are mapped to actions. This defines the state machine for a single call. For example, state change of terminated leads to action of "freeUpBandwidth".
State changeState(Call, CurrentState, ChangeToState)

Invoice Service:
Invoice createInvoice(call_id, talk_time, price_per_minute, currency)
addBalance(user_id, amount, currency)
lockBalance(user_id, amount, currency)
getBalance(user_id)

Router Service:
Provider getRoute(User A, User B)
Provider is a phone service provider's routing address. The call paths will now be patched using this address.
```

\
**0. Is this the final System Design?**

_No. However, this video shows what would be a good attempt at the problem._

_The final Calling App system design is more comprehensive and avoids the mistake of jumping at 'a' solution, instead of arriving at a good solution. The videos are in progress and should be ready in a few days._

**1. Can a call be made from a VOIP user to a PSTN user?**\
\
_Yes. The connection is 'bridged' on the switch. It's like taking data in two different languages and translating them for each side. If A is on VOIP and B on PSTN:_\
_VOIP data from A is translated to PSTN and sent to B._\
_PSTN data from B is translated to VOIP and sent to A._\
\
**2. How are calls routed in PSTN?**\
\
_In comparison with the internet:_\
_PSTN is a set of telephone routers. The Internet is a set of IP routers._\
__\
_Your phone sends a request to your network provider, which uses spectrum._\
_Whatsapp instead sends the request to your ISP, which uses fibre optic cables._\
\
**3. Will both the discussed approaches to designing a solution pass in an interview?**\
\
_Yes. The first one is acceptable by an engineer with less than 10 years of experience. The interviewer can help them along the way._\
_Topics like PSTN and VOIP calls are not dealt with by every engineer. The interviewer may explain the terms then._\
__\
_However, senior engineers are expected to be well versed with long running transactions, state machines and so on. They are also expected to know about networking protocols like SIP and VOIP._\
\
**4. Can I be expected to code some part of the system during the interview?**\
\
_Although not likely, an engineer can be asked to code some parts of the system. This will be discussed further in the LLD videos of the series._\
\
**5. Does every component of the system need an explanation?**\
\
_Sometimes you can mention what a service does and treat it as an abstraction. Mentioning real-world implementation examples help:_\
\
a) Service Discovery → Built using Zookeeper\
b) Recommendation Engine → Kafka can be used for data pipelines.\
&#x20;   (Hadoop is useful for batch processing. Spark for real time processing.)\
c) Telemetry and logging → ELK stack\
d) Gateway → Elastic Load Balancer\
e) Message Queue → Kafka\
\
**6. Is this extensible for video calls?**\
\
_A lot of the ideas can be extended to video calling. We will be talking about them in the coming videos._\
\
**7. What is the full form of PSTN? Which companies use it?**\
\
_It stands for Public Switched Telephone Network. The network is used to make phone calls across the globe, by multiple phone service providers._\
\
**8. Choice of databases? Can we use RDBMS at Whatsapp scale?**\
\
_An event driven system seems suitable here. That would require persistence of the log of events, and a view of the current state of each call._\
__\
_The log of events can be persisted in a file storage system like Hive (Cheap storage). The current/final state of ongoing calls can be stored in an RDBMS for fast queries._\
_The reason for choosing RDBMS is possible transaction guarantees required for a long running call._\
\
**9. Any security issues or encryption to keep in mind?**\
\
_VOIP can be encrypted end-to-end using a secure transport protocol like SRTP. PSTN has been here for a long time, and listening in on a PSTN conversation requires physical access to the wires involved._\
__\
_That is difficult for anybody other than the government to do. Some governments make recording of calls from suspicious parties a mandate._\
\
**10. What if I want to add a caller-tune? Where should it go?**\
\
_We can store the audio files in the switch. If a user asks for a caller tune, we store a key-value pair of UserId -> mediaID. We then play the media file on the calling side when the receiver is called at the switch, till we hit the 'pickup' state._

Internet Backbone: [https://en.wikipedia.org/wiki/Internet\_backbone](https://en.wikipedia.org/wiki/Internet\_backbone)

PSTN: [https://en.wikipedia.org/wiki/Public\_switched\_telephone\_network](https://en.wikipedia.org/wiki/Public\_switched\_telephone\_network)

WhatsApp System Design video: [https://youtu.be/vvhC64hQZMk](https://youtu.be/vvhC64hQZMk)&#x20;

We model the call's lifecycle as a state machine, and move both state storage and transition logic to the Call State Manager.

The state machine: [https://en.wikipedia.org/wiki/Finite-state\_machine](https://en.wikipedia.org/wiki/Finite-state\_machine)

We decide how the billing service should handle call invoicing, decoupling services and polling for call termination.

&#x20;Consistent Hashing on whiteboard: [https://www.youtube.com/watch?v=zaRkONvyGr8](https://www.youtube.com/watch?v=zaRkONvyGr8)

![](<.gitbook/assets/image (29).png>)

![](<.gitbook/assets/image (30).png>)

![](<.gitbook/assets/image (31).png>)

![](<.gitbook/assets/image (32).png>)

{% embed url="https://youtu.be/2wwTF4JZUlA" %}

__
