# API Architecture

## A visual history of web API architecture <a href="#51d7" id="51d7"></a>

![](https://miro.medium.com/max/3600/1\*1J\_wUEY8RQyxrlMokTxRXw.png)

To be fair they are both beautiful buildings! Photo by [Jene Yeo](https://unsplash.com/@jeneyeo) on [Unsplash](https://unsplash.com/s/photos/new-york-architecture) and [Shinya Suzuki](https://www.flickr.com/photos/shinyasuzuk).

So, what’s the architecture of your web application? It sounds like a deceptively simple question — maybe even a softball first-round interview question.![](https://miro.medium.com/max/960/1\*RPn\_vC7cM\_3NzZjuljpCtg.gif)

Turns out, after nearly 30 years of the Internet, the answer is far from straightforward, as [page one of a Google search](https://www.google.com/search?q=web+application+architecture) for the above term reveals:![](https://miro.medium.com/max/6308/1\*S-1B\_3MteRz-x8dTxBULKQ.png)![](https://miro.medium.com/max/960/1\*Tg4806i0NYXrZRem0H016A.gif)

We should start by clarifying what we mean by the word “architecture.” At its most basic, this is a question whose answer is just a picture and some arrows.

> At its most basic, software architecture is just simple shapes with arrows — just a map and a path

The [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) looks like this:![](https://miro.medium.com/max/60/1\*DCXtTdOyywZc-iNfcaDEuQ.png?q=20)![](https://miro.medium.com/max/772/1\*DCXtTdOyywZc-iNfcaDEuQ.png)

Android app architecture, as [proposed by Google](https://developer.android.com/jetpack/guide), looks like this:![](https://miro.medium.com/max/60/1\*s0nYHStEJPTrQqTLQ6zSzg.png?q=20)![](https://miro.medium.com/max/960/1\*s0nYHStEJPTrQqTLQ6zSzg.png)

What about web application architectures? Surely something as straightforward as a web application — one of the foundational pieces of software we were building back in the mid 1990s — will be a well-documented victory lap of classes, hierarchies and data flows by the 2020s?

## Ruby web app architecture <a href="#bf2d" id="bf2d"></a>

The godfather of modern, opinionated web application architecture is probably [Ruby on Rails](https://guides.rubyonrails.org) — so what happens when we go searching? (Maybe skip the [results](https://discuss.rubyonrails.org/t/effect-of-the-last-week-on-ruby-on-rails/77702) from April 2021 😬). Not a lot of [helpful results](https://www.google.com/search?q=ruby+on+rails+application+architecture):![](https://miro.medium.com/max/2560/1\*2o0hS6isiWa8rtiC242Y0A.png)

The Internet generally understands Rails architectures to be based on [MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller), though a diagram like this doesn’t offer much in the way of guidance in how to construct even a moderately complex app. The [Rails Guides](https://guides.rubyonrails.org) site isn’t a guide so much as an index of every class in the Rails universe, with little in the way of explanation about how they fit together. Here is [a more recent take as of 2017:](https://1sherlynn.medium.com/ruby-on-rails-beginner-adventures-simple-blog-application-6dffd6dcb11a)![](https://miro.medium.com/max/1436/1\*6LRNfjJYwRIa49QzZHb3qQ.png)

Even then, this seems to suggest that our app requires 3 (maybe 4?) classes, dominated by Action controllers and Active Records (aka models).

## JavaScript web app architecture <a href="#103e" id="103e"></a>

By now, hopefully you have gotten the memo about the JavaScript event loop. No? Don’t worry, because every [nodejs architectural diagram](https://scoutapm.com/blog/nodejs-architecture-and-12-best-practices-for-nodejs-development) is just this:![](https://miro.medium.com/max/60/1\*CLlgbgHf2sFIHaNtfJBUGw.png?q=20)![](https://miro.medium.com/max/871/1\*CLlgbgHf2sFIHaNtfJBUGw.png)

What is the architecture of your nodejs web API? **Event loop**.![](https://miro.medium.com/freeze/max/60/1\*g5y9RClG7Ab95qexsIPjkQ.gif?q=20)![](https://miro.medium.com/max/500/1\*g5y9RClG7Ab95qexsIPjkQ.gif)

We got it node. You’re proud of your event loop.

The remaining web results when [googling nodejs web application architecture](https://www.google.com/search?q=nodejs+web+application+architecture) are a hodgepodge of SEOd links for new developers learning to program, with a few shady certification programs thrown in the mix. I’d normally suggest you see for yourself, but I suspect these results have a higher-than-normal likelihood of giving you spyware.

## Java web app architecture <a href="#c203" id="c203"></a>

The Java world has a bit more maturity when it comes to describing application architecture, though Java was [widely](https://currents.google.com/105201233571140699617/posts/1QhcnQizuPc) [ridiculed](https://github.com/EnterpriseQualityCoding/FizzBuzzEnterpriseEdition) in the late 2000s and early 2010s for its perceived complexity and the overwrought nature of its [main development IDE](https://www.quora.com/Why-is-Eclipse-objectively-such-a-bad-program), [Eclipse](https://movingfulcrum.com/the-fall-of-eclipse/).![](https://miro.medium.com/freeze/max/60/1\*MzEMpKy8zuhs47Aut5wLxA.gif?q=20)![](https://miro.medium.com/max/384/1\*MzEMpKy8zuhs47Aut5wLxA.gif)Or any IDE, really

This [ancient blog](https://www.javatpoint.com/spring-boot-architecture) about Spring Boot shows at least 4 layers with perhaps 6–7 different class types:![](https://miro.medium.com/max/60/1\*G5OSR4nY3BI09b5RclstWg.png?q=20)![](https://miro.medium.com/max/500/1\*G5OSR4nY3BI09b5RclstWg.png)

More [recent blog posts](https://openclassrooms.com/en/courses/5684146-create-web-applications-efficiently-with-the-spring-boot-mvc-framework/6156961-organize-your-application-code-in-three-tier-architecture) have tried to rationalize this layered approach with the Rails MVC architectural approach:![](https://miro.medium.com/max/52/1\*i3AkLWlXQUI4yUxBhVFzYg.png?q=20)![](https://miro.medium.com/max/426/1\*i3AkLWlXQUI4yUxBhVFzYg.png)

O’Reilly predictably gives us [a more disciplined discussion](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html) of Java EE architecture:![](https://miro.medium.com/max/60/1\*heO0ui8hGVpTL2xZoAlxlA.png?q=20)![](https://miro.medium.com/max/1376/1\*heO0ui8hGVpTL2xZoAlxlA.png)

But it still feels like we are a step away from answering the [meat-and-potatoes problems](https://idioms.thefreedictionary.com/meat+and+potatoes#:\~:text=Concerned%20with%20or%20pertaining%20to,unfairly%20target%20lower%2Dclass%20workers.) of software developers tasked with building web apps and APIs.

> What are the classes I should expect to write when building a web application or API, and how do they fit together?

## Big Company Web Application Architecture <a href="#eee1" id="eee1"></a>

![](https://miro.medium.com/max/60/0\*o53cPnjS\_SSVpASi.jpg?q=20)![](https://miro.medium.com/max/1000/0\*o53cPnjS\_SSVpASi.jpg)

Can we look to the giants for guidance? Microsoft has plenty to say about [common web application architectures](https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures). So much so, in fact, they helpfully included **14 diagrams** to aid your understanding.![](https://miro.medium.com/max/4000/1\*oCFZnXrvifXj-LHIgATciA.png)

To be fair, Microsoft does have a number of smart points to make, including use of Clean architecture, dependency inversion, and the importance of testability. However, those points are buried so deep under jargon about ASP.NET and Azure deployment models that it largely passes unnoticed amongst the non-MSFT developer world.

Also, they need more animated GIFs…![](https://miro.medium.com/max/6624/1\*yrmV0ZimLXQ\_lhOGy9TZBw.png)Too. Many. Words.![](https://miro.medium.com/max/1500/0\*1hOHu9EWsgmscYNx)

Uber recently [documented its API approach](https://eng.uber.com/architecture-api-gateway/), with the following stack:![](https://miro.medium.com/max/1564/0\*u--wSsomG2Rcy0EU.png)

This approach comes closest, I believe, to offering practical advice, but it’s predicated on big-company patterns like [microservices](https://dwmkerr.com/the-death-of-microservice-madness-in-2018/).![](https://miro.medium.com/freeze/max/60/1\*s5Hnl8GnbxqXsIgFQgowpA.gif?q=20)![](https://miro.medium.com/max/480/1\*s5Hnl8GnbxqXsIgFQgowpA.gif)Car = Microservice

As an example, _“**Client** performs a request to a back-end service…Users can configure the internal functionalities of a client, like a request and response transformation, validation of schema, circuit breaking and retries, timeout and deadline management, and error handling”_.

So…how do you write the “back-end service?” And doesn’t the service have the same considerations (validation, parameter extraction, etc) of this top-level API gateway? According to [Conway’s law](https://en.wikipedia.org/wiki/Conway's\_law), your API structure is your team structure.

Most companies aren’t going to have dedicated teams for each aspect of their API, so the “Client” layer described by Uber probably won’t exist at your company — it will be a “Service” layer that does the thing the API is supposed to do, and we need to know, now, how to design that Service.

## Why is this so hard? <a href="#a24e" id="a24e"></a>

The truth is — most web applications are small, and their architectures don’t matter. They’re going to be rewritten every N years, so they emerge as a [monolith](https://en.wikipedia.org/wiki/Monolithic\_application) maintained by one or a handful of developers…**and it’s fine**.![](https://miro.medium.com/freeze/max/60/1\*yfy4s\_JZNlmP7\_KLPJlEPw.gif?q=20)![](https://miro.medium.com/max/500/1\*yfy4s\_JZNlmP7\_KLPJlEPw.gif)I’ll just rewrite it once Coinbase releases its JavaScript framework in 2025

Larger applications by FANG have teams devoted to every piece of the stack, plus tooling, security, source code management, etc. But what about developers and projects who want to build testable, maintainable, scalable, **large** production apps? What are the classes, data flows, and hierarchies that we should expect to build?

## Toward a Clean API Architecture <a href="#041b" id="041b"></a>

At [Perry Street Software](https://www.perrystreet.com), we have been building a large API for the past decade to power our family of [dating](https://www.scruff.com) [apps](https://www.jackd.com). We recently have devoted time to codifying and upgrading our server API architecture in a way that actually maps quite closely to the [Clean architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html), with inspiration from functional/[reactive programming](http://reactivex.io) paradigms that have (rightly) become [popular of late](https://www.quora.com/Why-has-reactive-programming-become-so-popular).

We call this our **Clean API Architecture**. Before we begin, however, we are going to need to take a brief detour into the execution flow of typical web applications. Remember, any architecture is just a picture and some arrows, a map and a path.![](https://miro.medium.com/max/60/0\*heYl6rAQtp8V0CN\_?q=20)![](https://miro.medium.com/max/612/0\*heYl6rAQtp8V0CN\_)The island is code; the red line is an execution path; the map is an Architecture.

Too often the line and/or the arrows are skipped right over — we should pause to explore the nature of the arrows themselves.



In this series we are exploring patterns for [web API architecture](https://itnext.io/a-visual-history-of-web-api-architecture-c36044df2ac7). In order to build our case, we must first take a detour to explore common patterns of code execution whenever you call a remote API endpoint.

## Computation 101 <a href="#62c3" id="62c3"></a>

![](https://miro.medium.com/max/960/1\*r5g5eNQVkRtors1kmBlk5Q.gif)Never hurts to refresh the basics

When code is executed, it passes through a series of layers, each of which takes some amount of time, until each layer has run to completion and returned, and a value is ultimately presented to the user.

Normally, each of these steps takes a consistent amount of time, unless one of these layers happens to be [i/o bound ](https://en.wikipedia.org/wiki/I/O\_bound)— it is hitting network, disk, or a database.

## V-shaped execution <a href="#ca9a" id="ca9a"></a>

Regardless of your application architecture — 1, 2, 3, or 10 layers, all “happy path” execution basically looks like a “V” shape. A call stack is built, probably culminating in a data access request from a database (or caching layer), and that data is returned, massaged, and delivered to the client via an HTTP response.![](https://miro.medium.com/max/4448/1\*M-jvJhKsLKUNLCWErK4ZQA.png)

We call this the **V shaped execution path**. For small web apis, modeling your execution this way works pretty well. As your application begins to scale, however, the V becomes harder to maintain for all but the most trivial of endpoints.

## U-shaped execution <a href="#e39d" id="e39d"></a>

At a certain scale your execution looks more like this:![](https://miro.medium.com/max/4448/1\*Vkvyw5Jz6vm2KGtieqcVWg.png)

As an app scales, write operations to relational databases will soon become your bottleneck. Though it’s also possible that read operations can cause slowdowns, generally you can just “put a cache in front of it” and speed things up.

It should be intuitive that relational write operations are a bottleneck — the ACID guarantees provided by relational storage systems like Mysql and Postgres are costly, and as more and more users write to the same table or tables, those systems will slow. Services like [AWS Aurora](https://aws.amazon.com/rds/aurora/) have raised the bar of performance for relational systems, but limits still exist. For our company, our relational database service, powered by AWS Aurora, is one of the most expensive components in our AWS bill each year, driven primarily by [Database Storage and IOs](https://aws.amazon.com/rds/aurora/pricing/?pg=pr\&loc=1).

Ultimately, web application performance starts to resemble a “U”, not a “V”, when your app waits on the results of a database operation. Even if you have multithreaded or non-blocking application servers like [nodejs](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/), your application code will still be waiting for a response from the database before providing the user with a response to his POST, PUT or DELETE operation.

## W-shaped execution <a href="#1e5a" id="1e5a"></a>

![](https://miro.medium.com/max/768/1\*YWkaq9jBTjkgJ4qLDbRlsQ.gif)

At a certain point it becomes clear that, for consistent, reliable, and resilient endpoints, any state mutation requires a queue and an asynchronous confirmation. It is the job of every endpoint to get data back to the caller as soon as possible. In the event of a GET operation, once data is retrieved, that operation is concluded. In the event of a POST, PUT or DELETE operation, that endpoint should again respond to the caller as quickly as possible with a promise of an additional, later callback, likely over a websocket, with the results of that state mutation.

Indeed, this is the exact [architecture of gRPC](https://www.grpc.io/docs/what-is-grpc/core-concepts/#server-streaming-rpc), Google’s remote procedure call framework. Clients make calls and the server responds with 1 or many messages, until the channel is closed. While our API does not (yet) use gRPC, the basic need for a communication loop is at the core of any modern, high performance web API.

Below you can see an example of what a basic single-response operation might look like:![](https://miro.medium.com/max/4448/1\*ex41Jc3CUjALVIMB-1-hjA.png)

As part of the response to the initial request, we additionally fire off a new asynchronous process that itself may invoke more business logic and data layer operations, with the results ultimately delivered via an asynchronous protocol like [websockets](https://en.wikipedia.org/wiki/WebSocket#:\~:text=WebSocket%20is%20a%20computer%20communications,WebSocket%20is%20distinct%20from%20HTTP.).

In order to have an “architecture” to your code, you need a map and a path, a diagram and some arrows. But what, exactly, do we mean by a “path” — what is the nature of the arrows and how do they behave?

Put another way — as we pass data between the layers, what are we passing? In a simple architecture, it is likely that you would be passing a parameters hash down the layers:![](https://miro.medium.com/max/1680/1\*oKgwW\_fcR8GLj1dnWoxMyQ.png)

And receive a model object up the layers![](https://miro.medium.com/max/60/1\*1m0R1nEBihnfkBWaldRw-g.png?q=20)![](https://miro.medium.com/max/840/1\*1m0R1nEBihnfkBWaldRw-g.png)

Which eventually gets mapped to some sort of HTTP response code + a response body.

This strategy works for simple APIs, but what happens when we start to have errors?![](https://miro.medium.com/freeze/max/60/1\*UVxc-myPyezubmgSJoKV9Q.gif?q=20)![](https://miro.medium.com/max/245/1\*UVxc-myPyezubmgSJoKV9Q.gif)Follow the (functional) road!

## Handling errors in imperative code <a href="#5e84" id="5e84"></a>

Imagine we have a simple API endpoint, `post ‘/user/email’`. We want to update the email address of a customer and notify him that it has been changed.

You might imaging the following method in our v1 quick-and-dirty method (implemented here in [Sinatra](http://sinatrarb.com)/Ruby):

OK, but how do we handle errors here? We need to check if it is valid:

We also want to check if there is no customer record:

What if the database fails to do its write operation?

And what if our smtp service fails to notify about the change? Maybe we want to log that fact?

With imperative code, error handling is critical, and as APIs scale more errors and edge cases become possible or visible. Error handling in imperative code makes the code much harder to read.

## Railway oriented programming <a href="#01af" id="01af"></a>

_(Note: this section adapted from a presentation by_ [_F# for Fun and Profit_](https://fsharpforfunandprofit.com)_)_![](https://miro.medium.com/freeze/max/60/1\*MqNvP5ci\_BN4rT4Jr5Jadg.gif?q=20)![](https://miro.medium.com/max/340/1\*MqNvP5ci\_BN4rT4Jr5Jadg.gif)MS actually has a lot of [good](https://en.wikipedia.org/wiki/ReactiveX) [ideas](https://en.wikipedia.org/wiki/TypeScript).

In 2014, a [developer from Microsoft](https://twitter.com/ScottWlaschin) coined the term “[Railway Oriented Programming](https://fsharpforfunandprofit.com/rop/)”, which derived much inspiration from functional programming methods. He rightly pointed out the importance of “designing the unhappy path” and thinking about functions with more than one output.

When we first model endpoints, we think about them as simple functions with one input and one output:![](https://miro.medium.com/max/60/1\*FJKq9HPqeFi01IcKU8MGAw.png?q=20)![](https://miro.medium.com/max/1264/1\*FJKq9HPqeFi01IcKU8MGAw.png)

As the endpoint increases in scope, we may think to break it apart into various steps, layers, components, etc:![](https://miro.medium.com/max/60/1\*3hvIUrV2bj25P4EBYfp9-w.png?q=20)![](https://miro.medium.com/max/1264/1\*3hvIUrV2bj25P4EBYfp9-w.png)

But in the real world, we learn that each of these steps has the potential to fail, and we have to provide mechanisms for the interruption of control flow at each of these steps:![](https://miro.medium.com/max/60/1\*rKqZc1NTrposNA4emtrE7g.png?q=20)![](https://miro.medium.com/max/1264/1\*rKqZc1NTrposNA4emtrE7g.png)

The realization that we need functions that return **multiple values**, either success or error responses, is the foundation of railway oriented programming and the basis of our API design today.

## Success and Failure <a href="#4d36" id="4d36"></a>

![](https://miro.medium.com/freeze/max/60/1\*Z0QMxWCT7eTpXEe9HZxn0w.gif?q=20)![](https://miro.medium.com/max/400/1\*Z0QMxWCT7eTpXEe9HZxn0w.gif)Errors are Responses too

A failure can occur anywhere in our processing chain, and we needed a streamlined way to exit processing and return a **Failure** to the client. Or, if everything is good, to return a **Success**.

The series of classes, or steps used to process the endpoint represents a functional unit, which always results in a **Success** or **Failure,** which contain resulting data (for a successful read operation for example) or information about the failure that occurred (such as during validation).

In turn, each step can return a **Success** or **Failure,** which the orchestrator (the **Controller**) uses to stop or continue processing.![](https://miro.medium.com/max/60/1\*0MY9oKDrciUPf-L7Cn\_EAw.png?q=20)![](https://miro.medium.com/max/1180/1\*0MY9oKDrciUPf-L7Cn\_EAw.png)

## Railway Oriented Programming <a href="#bd12" id="bd12"></a>

We visualize the functional processing above as two parallel railroad tracks. For a completely successful request, the train proceeds along the green (**Success**) track until finished. However, if a failure occurs anywhere along the way, the train switches to the red (**Failure**) track, and we return the error to the client.![](https://miro.medium.com/max/60/1\*OYWW-5en-77WJLsLOAq4CQ.png?q=20)![](https://miro.medium.com/max/1794/1\*OYWW-5en-77WJLsLOAq4CQ.png)

The construction and composition of two-track functions is based on theories in functional programming. There are number of important rules that govern how to build and compose “two-track functions”. We suggest reading through all of the slides at: [https://fsharpforfunandprofit.com/rop/](https://fsharpforfunandprofit.com/rop/)

## Building two-track in Ruby <a href="#2051" id="2051"></a>

![](https://miro.medium.com/max/60/1\*mq8JkpZegvulu-Z0dQ0LIA.jpeg?q=20)![](https://miro.medium.com/max/512/1\*mq8JkpZegvulu-Z0dQ0LIA.jpeg)Haskell humor. See [Stack Overflow.](https://stackoverflow.com/q/3870088)

To build a two-track execution path we use [dry-monads](https://dry-rb.org/gems/dry-monads/1.3/). Dry monads defines monads, which is a formal name for a special [ruby mixin](https://www.geeksforgeeks.org/ruby-mixins/) that defines two new [Result types](https://dry-rb.org/gems/dry-monads/1.3/result/): Success and Failure

Now, any time we might have returned a value, such as an [ActiveRecord](https://guides.rubyonrails.org/active\_record\_basics.html) model object, we now will return a `Success(ActiveRecord)`

Moreover, whereas we may have called raise `SomeError` or even `halt 4XX`, we now will return a `Failure(SomeError)`

The documentation for dry-monads is [described here](https://dry-rb.org/gems/dry-monads/1.3/), but the final form can be seen here:

In this implementation, you can see that find\_user now returns a monad, not a user directly. The bind method unwraps success values and makes them available to the blocks. With additional metaprogramming magic thanks to ruby, we can simplify this block even further:

## Relationship to Reactive client programming <a href="#c3cc" id="c3cc"></a>

Thus far we have been talking about server-side architecture, but it is worth noting that the principles we are describing have a direct mapping to client-side architectural patterns as described by [Reactive programming](http://reactivex.io).

In Swift, we could rewrite that code block in [Combine](https://developer.apple.com/documentation/combine), which has the concept of a [Publisher](https://developer.apple.com/documentation/combine/publisher), which defines `Output` and `Failure` types similar to dry-monads above:

In Kotlin, we could rewrite that code block in [RxJava](https://github.com/ReactiveX/RxJava) using the [Single](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html) operator, which again defines both value and error outputs:

## Updating our W-shaped execution <a href="#f7d4" id="f7d4"></a>

In a previous blog post we described [patterns of web API execution flows](https://medium.com/p/9d65232e8a24/). The final pattern we recommended, W-shaped execution, can be updated in the following way:![](https://miro.medium.com/max/4448/1\*J9VnyEblecyQbxqyZ0pZfA.png)Two lanes on each path — one for success, one for failure

We know that, in the event we transition to an error state in one of those layers, we will remain on the “Error” track until we ultimately return a value to the user via a POST (or potentially, though not necessarily always, in a Websocket).

## Next up <a href="#ddb3" id="ddb3"></a>

It’s time we start defining some layers! Taking inspiration from Clean architecture and using the principles of V-U-W execution flows and railway oriented programming, we are ready to [define the architecture](https://medium.com/p/2b57074084d5/) that we use today for our API endpoint design.

We started this 6-part [series on how to build web APIs](https://itnext.io/a-visual-history-of-web-api-architecture-c36044df2ac7?source=your\_stories\_page-------------------------------------) by introducing the variety of architectures that have been proposed or put into use by various languages and frameworks over the years. Among the most commonly discussed architectures online is the [Clean architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html), which aspires to produce a separation of concerns by subdividing a project into layers. Each layer abides by the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility\_principle#:\~:text=The%20single%2Dresponsibility%20principle%20\(SRP,it%20should%20encapsulate%20that%20part.\&text=Hence%2C%20each%20module%20should%20be%20responsible%20for%20each%20role.), ensuring each class is only handling one part of the process, and is more easily and thoroughly unit tested.![](https://miro.medium.com/max/1544/1\*DCXtTdOyywZc-iNfcaDEuQ.png)

## Applying Clean to Endpoints <a href="#bd66" id="bd66"></a>

The Clean architecture can be used in many domains. In another blog series we describe how we applied Clean to our [mobile applications](https://medium.com/swlh/kotlin-in-xcode-swift-in-android-studio-26a4ace6fc72). Today, we are going to talk about how we apply Clean to API endpoints. We call this the **Clean API Architecture:**![](https://miro.medium.com/max/56/1\*yTDpfIqqAdeKRhbHwfhrYQ.png?q=20)![](https://miro.medium.com/max/1822/1\*yTDpfIqqAdeKRhbHwfhrYQ.png)

We have the following layers (and colors) which map to the original Clean architecture:

🔵 **FRAMEWORKS & CLOUD**\
↓\
**🟢 INTERFACE ADAPTERS**\
↓\
**🔴 APPLICATION LOGIC**\
↓\
**🟠 ENTITY LOGIC**\
↓\
**🟡 DATA**

> None of the layers have visibility into higher layers. They may have references to their child layer, but definitely not their grandchildren

This diagram also depicts our [W-shaped execution flow](https://medium.com/p/9d65232e8a24/) we described in an earlier blog post, this time represented by arrows that start both at the HTTP layer and again at the Interface Adapter layer from a Queued asynchronous job.

> Now it’s time to show you the classes of our architecture.

![](https://miro.medium.com/freeze/max/60/1\*XuGmz2t1JHYnrBxGAARnkQ.gif?q=20)![](https://miro.medium.com/max/460/1\*XuGmz2t1JHYnrBxGAARnkQ.gif)You had me at Clean

## 🔵 Frameworks <a href="#c64e" id="c64e"></a>

![](https://miro.medium.com/max/56/1\*4k0KqqGUQbvvDaBuQogNFQ.png?q=20)![](https://miro.medium.com/max/1822/1\*4k0KqqGUQbvvDaBuQogNFQ.png)

Any endpoint request must be routed to the appropriate code path through a **Load Balancer, Web Server, Application Server,** and an **API / Web Framework.** (We use [Sinatra](http://sinatrarb.com) for this last part, but popular frameworks include [Rails](https://rubyonrails.org), [Django](https://www.djangoproject.com), [Spring Boot](https://spring.io/projects/spring-boot), and [others](https://cakephp.org)). API frameworks offer the [most](https://guides.rubyonrails.org) [documentation](https://docs.djangoproject.com/en/3.2/) [online](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/) and is a common (the most common?) architectural structure.

Once a request reaches your API or web framework, however, patterns can diverge widely.

Frameworks like Rails employ an [MVC](https://medium.com/stackavenue/m-v-c-architecture-of-ruby-on-rails-9712719a69ed#:\~:text=Ruby%20on%20Rails%20uses%20the,the%20user%20interface%20and%20application) pattern that works well for smaller projects, but have weaknesses when it comes to large, high availability APIs such as ours. Rails models and controllers get [fat](https://codeclimate.com/blog/7-ways-to-decompose-fat-activerecord-models/) [very](https://medium.com/@jaysadiq/refactoring-a-fat-rails-model-dc3cfda64d22) [quickly](https://www.rubypigeon.com/posts/rails-models-bloated-should-i-use-concerns/) when applied to large APIs.![](https://miro.medium.com/freeze/max/60/1\*amVdu6huQGS5ScLhMVCRnA.gif?q=20)![](https://miro.medium.com/max/500/1\*amVdu6huQGS5ScLhMVCRnA.gif)The inevitable course of a Rails model class

Consequently, everything that comes after the Frameworks + cloud layer is (mostly) novel and inspired by the Clean architecture. We will be making the case for our architecture and our decisions in the remainder of this series.

### Supporting classes <a href="#84ff" id="84ff"></a>

> In any given layer, we will have one or more supporting classes. These are classes that are going to be single-purpose and have no references to other layers above or below them. They help with code re-use and duplication, and enable us to avoid writing complex god classes in a given layer.

We include cloud services as supporting classes of our Framework layer. AWS services like [EC2](https://aws.amazon.com/ec2/), [SQS](https://aws.amazon.com/sqs/),[ RDS](https://aws.amazon.com/rds/) and [ElastiCache](https://aws.amazon.com/elasticache/) are supporting classes — NOT “inner” or central layer — because, as [others have pointed out](https://pusher.com/tutorials/clean-architecture-introduction), the UI and the database depend on the business rules, but the business rules don’t depend on the UI or database.

## 🟢 Interface adapters <a href="#1f77" id="1f77"></a>

![](https://miro.medium.com/max/56/1\*-QcER\_dCsE9gLmA51h\_ulA.png?q=20)![](https://miro.medium.com/max/1822/1\*-QcER\_dCsE9gLmA51h\_ulA.png)

Once a request comes in via our framework, a `Controller` orchestrates the processing of the endpoint by invoking a `Request` object to extract each parameter, validate its syntax, and authenticate the user making the request.

Controllers are the first place our application code is introduced. Controllers instantiate our classes and move data between classes in the 🔴 **Application Logic** layer.

It’s important to note that Controllers are not the only orchestration object in the Interface adapter layer. We also have `Jobs`, which are used in our asynchronous queue processing layer (more to come).

### Supporting classes <a href="#f56a" id="f56a"></a>

Controllers depend on classes including `Validators`, which check the syntax of incoming data, and `Presenters`, which format outgoing data, and `Response` objects, which map objects and/or hashes into JSON, HAML and other formats.

`Socket relay` classes communicate state changes to the client over a socket communication channel, such as Websockets.

Request classes are typed data structures that bring together the necessary components for the request being made. This is different from standard HTTP requests, which (assuming CGI) are made up of key/value string pairs.

Response classes are like [renderers](https://guides.rubyonrails.org/layouts\_and\_rendering.html) in Rails, and enable you to return HAML, JSON or other types.

Parameter extractors extract data out of [the params hash](https://medium.com/launch-school/params-in-rails-where-do-they-come-from-b172cdb46eb4) and converts it to properly typed values, such as ints, floats and strings.

## 🔴 Application Logic <a href="#a661" id="a661"></a>

![](https://miro.medium.com/max/56/1\*iCR9R52sI-V5nwPdLXaX8Q.png?q=20)![](https://miro.medium.com/max/1822/1\*iCR9R52sI-V5nwPdLXaX8Q.png)

GET request made to read endpoints are next passed to the **Application Logic** layer where a `Service` ensures the validity of the inputs, makes sure the user is authorized to access data, and then retrieves data from the **Entity Logic Layer** through a `Repo` (for databases) and/or `Adapter` (for APIs). Service objects return `Result` objects as defined by dry-monads.

POST, DELETE and PUT requests made to write endpoints do the same thing as read endpoints, but defer processing by enqueuing `Service` inputs through our queue — Amazon SQS — and write the data to the **Entity Logic Layer** through a `Job` or `Service`**.**

Jobs are used to orchestrate side effects, such as sending a socket message through a **Relay** after a data mutation completes

### Supporting classes <a href="#04c2" id="04c2"></a>

In our implementation of the Clean API architecture, the `Service` class itself assembles a distinct collection of `Validator` classes to provide an additional layer of semantic validation for a request. Thus, we have two layers of validation — syntactic, happening via the `Request` layer, and semantic, happening via the `Service` .

## 🟠 Entity logic <a href="#623f" id="623f"></a>

![](https://miro.medium.com/max/56/1\*ps7fHlT-nb1FNtJoQoBlhg.png?q=20)![](https://miro.medium.com/max/1822/1\*ps7fHlT-nb1FNtJoQoBlhg.png)

Entity logic refers to components that are common not only to this endpoint, but others as well. `Repository`classes, which provide us access to persistent stores like Mysql or Postgres databases, and `Adapter` classes, which provide us access to APIs, including AWS storage apis like S3, ElastiCache and others. We expect classes in this layer to be used over and over again; classes in the layer above are often single-purpose to an endpoint.

## 🟡 Data <a href="#fba0" id="fba0"></a>

![](https://miro.medium.com/max/56/1\*mUiSz0P4xZlHgdu411CsTw.png?q=20)![](https://miro.medium.com/max/1822/1\*mUiSz0P4xZlHgdu411CsTw.png)

This is ideally a very simple layer that provides an actual interface into our different storage systems. If you use Rails you will likely be receiving [ActiveRecord](https://guides.rubyonrails.org/active\_record\_basics.html) objects; APIs are ideally returning [ruby structs](https://brewhouse.io/2015/07/31/be-nice-to-others-and-your-future-self-use-data-objects.html).

## Testing <a href="#cb2d" id="cb2d"></a>

In other domains — such as Android or iOS development — we have created interfaces to our data storage layer. We use [dependency injection](https://www.freecodecamp.org/news/a-quick-intro-to-dependency-injection-what-it-is-and-when-to-use-it-7578c84fa88f/) so that, when we run tests, we are doing things like creating mocked, in-memory versions of SQLite data storage, rather than using real filesystem-backed data storage systems.

On our webserver, because of the dynamic nature of ruby, we use [stub\_const](https://relishapp.com/rspec/rspec-mocks/docs/mutating-constants) to overwrite singleton cloud services with mocked versions, or we will point singleton cloud services to local docker containers running Redis, Memcached, Mysql, etc.

Storage systems that power the Data layer, such as Mysql and Postgres, that are implicitly at the bottom of the diagram are very likely going to be process-wide singletons that are ideally injected or mocked. ActiveRecord maintains its [connection pool](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/ConnectionPool.html); systems Redis and Memcached will also likely need some kind of global pool or singleton managing access.

Stateless HTTP-based APIs, such as S3, DynamoDb, and others, are typically going to be mocked by [instance\_doubles](https://relishapp.com/rspec/rspec-mocks/v/3-1/docs/verifying-doubles/using-an-instance-double) or overridden connection parameters that point to local mocks.

## 🙄 Isn’t this overkill? <a href="#e08b" id="e08b"></a>

![](https://miro.medium.com/freeze/max/60/1\*9whHErtib5YpcTnxE\_vv8A.gif?q=20)![](https://miro.medium.com/max/306/1\*9whHErtib5YpcTnxE\_vv8A.gif)We promise this won’t lead to another [one](https://docs.spring.io/spring-framework/docs/2.5.x/javadoc-api/org/springframework/aop/config/SimpleBeanFactoryAwareAspectInstanceFactory.html) of [these](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/aop/framework/AbstractSingletonProxyFactoryBean.html).

We know [what you’re thinking](https://github.com/EnterpriseQualityCoding/FizzBuzzEnterpriseEdition). Isn’t this just another example of extra engineering and complexity that I don’t need, and that maybe you don’t need either?

Let’s explore this for a minute or two. Imagine the world’s simplest API — adding a favorite for a profile. This is what it might look like in a simple endpoint:![](https://miro.medium.com/max/60/1\*qI8vmqYX4XL8y7hEnJiAmw.png?q=20)![](https://miro.medium.com/max/1417/1\*qI8vmqYX4XL8y7hEnJiAmw.png)

In fact, this endpoint is implicitly relying on each of the layers we have defined above. Here is how:![](https://miro.medium.com/max/60/1\*thoz8Gb9oFi0e4pfJvNlvw.png?q=20)![](https://miro.medium.com/max/1441/1\*thoz8Gb9oFi0e4pfJvNlvw.png)

The first line, `post ‘favorite’ do`, is hook into Sinatra 🔵 framework, as you might expect.

The last line is a form of presentation logic that is part of our 🟢 Interface Adapter layer. It is simply a `200` response with no body.

What about the other layers in our Clean Api architecture? Can we see echos of those in this 6-line snippet?

### 🟢 Request <a href="#609b" id="609b"></a>

Params are passed directly without any extraction, and together the two symbols — `:target_id` and `:creator_id` — form an implied `Request`, comprising a `target_id` and `creator_id`.

### 🟢 Controller <a href="#7a05" id="7a05"></a>

Because of the lack of any validation or presentation customizability, there is no need for a controller so there is no equivalent in this code.

### 🔴 Service <a href="#4df0" id="4df0"></a>

The `where` clause takes your request — a target\_id and creator\_id — and finds a domain object — a `Favorite`.

### 🟠 Entity Logic <a href="#7124" id="7124"></a>

The `first_or_create` provides the Entity logic — interacting with persistent storage via a special ActiveRecord method

### 🟡 Data <a href="#4ce5" id="4ce5"></a>

The `Favorite` model is equivalent to your Data layer, providing a definition for the object being used.

Each of these steps we outlined in our Clean API Architecture is still happening in our simplified example, they are just happening implicitly. As a result, if we stick to our simple example, it makes things harder to test, and more importantly, harder to decompose as logic grows. How do you add more validations? Modify more classes? Add push or socket notifications? Endpoints become dozens, hundreds of lines long, and thus responsibilities become scattered throughout the code. We also fail to explain what happens if we encounter errors or how we handle authentication.

> The takeaway is clear: even the simplest APIs have to make these architectural choices. In the absence of explicit classes and rules, these choices are either assumed, implied or ignored, but they cannot be avoided.

## Next up: responsibilities of an endpoint <a href="#875c" id="875c"></a>

We’ve hopefully convinced you that this isn’t quite as easy as you might have first thought. Before we show how to build an endpoint, lets spend time rigorously documenting everything we expect our endpoint to do for us, and [connect those responsibilities back](https://medium.com/nerd-for-tech/the-endpoint-responsibility-checklist-d7763449f44a) to our Clean API Architecture.

## What does an API endpoint do? <a href="#a9bb" id="a9bb"></a>

In our series on [Clean API Architecture](https://medium.com/p/2b57074084d5/), we have explained how there are multiple layers involved in responding to an HTTP request. Broadly speaking, there are two kinds of requests — the kind that mutate state by writing to a database (POST/DELETE/PUT), and the kind that merely return state from a database (GET).

_(For brevity, when we say POST from now on, we implicitly mean POST/DELETE/PUT)._

For both GET and POST endpoints, you must fulfill the following responsibilities:

## Endpoint responsibility checklist <a href="#ea9a" id="ea9a"></a>

* 🔲 **RESTful Routing** (Which endpoint should we execute based on verb and URL pattern?)
* 🔲 **Control Flow** (Order our execution, also handle errors)
* 🔲 **Logging** (Write out request info for debugging and analysis)
* 🔲 **Parameter Extraction and Coercion** (Read inputs and store in variables)
* 🔲 **Syntactic Validation** (Are the inputs in the correct format?)
* 🔲 **Semantic Validation** (Do the inputs align with our business rules?)
* 🔲 **Authentication** (Which profile is making this request?)
* 🔲 **Authorization** (Does this profile have access to the requested data?)
* 🔲 **Presentation** (Format the data into the correct format for clients, if data is being returned)

## Write Endpoint Responsibilities Checklist <a href="#0354" id="0354"></a>

The write operations we listed earlier each need to update columns in an RDBMS like Mysql. As anyone who has managed RDBMS systems at scale can tell you, they can be unreliable — bad queries can cause slowdowns of other requests, schema changes can [lock tables](https://dba.stackexchange.com/questions/21075/way-to-prevent-queries-from-waiting-for-table-level-lock) for short or long periods of time, disk slowdowns lead to query slowdowns which lead to thread pool exhaustion.![](https://miro.medium.com/freeze/max/30/1\*xhzQh7-mDie-0bN3D12YNw.gif?q=20)![](https://miro.medium.com/max/384/1\*xhzQh7-mDie-0bN3D12YNw.gif)Your database when you have too many requests.

To prevent potential data loss and reduce the perceived user impact of each of these scenarios, in our architecture, all write operations are queued and performed by a task running on a separate machine (or in a separate cluster). We call these machines **DBWriter** machines**.**

Additional responsibilities of write endpoints, over and above what read endpoints are doing, include:

* 🔲 **Job Configuration** (Creating a hash with the necessary data to perform work in the background)
* 🔲 **Job Enqueuing** (Writing a message to a queue, like [AWS SQS](https://aws.amazon.com/sqs/))
* 🔲 **Queue Processing** (DBWriter processes that pull Job Configs from the queue and process Jobs)
* 🔲 **State mutation** (Writing to one or more data sources)
* 🔲 **Async Relay** (Returning write confirmations back to clients via WebSockets or chat cluster)

> We’ve identified no fewer than 14 separate responsibilities of an endpoint.

![](https://miro.medium.com/freeze/max/23/1\*o-D9rLO5V7aA0pqq80qU5g.gif?q=20)![](https://miro.medium.com/max/271/1\*o-D9rLO5V7aA0pqq80qU5g.gif)Yes this will be on the test

## Connecting it back to Clean <a href="#7369" id="7369"></a>

In our previous post we [discussed the Clean API architecture](https://medium.com/p/2b57074084d5/). We will now explain how and when in our Clean API architecture we address each of the responsibilities in the checklist above.

### Clean API for GET requests <a href="#d81d" id="d81d"></a>

Let’s start by visually identifying the execution path of a GET request in our Clean API architecture.![](https://miro.medium.com/max/28/1\*1jr1JjuOBVpJPwQZ1Zqgtw.png?q=20)![](https://miro.medium.com/max/1822/1\*1jr1JjuOBVpJPwQZ1Zqgtw.png)The path highlighted in yellow is for GET requests

The steps in this execution path can be summarized as:

**🔵Receive**→🟢**Validate (Syntactic)**→🔴**Validate (Semantic)**→🟢**Enqueue**→🟢**Respond**

The request is processed on an API server. Note the two stages of validation — one syntactic, one semantic. After validation, the job is enqueued and returns a success code to the client. In case of failure, no job is enqueued and an error code is returned to the client.

### Clean API for POST requests <a href="#b9b1" id="b9b1"></a>

![](https://miro.medium.com/max/28/1\*O5E4kEg5eMJKKCOz2jdnIw.png?q=20)![](https://miro.medium.com/max/1822/1\*O5E4kEg5eMJKKCOz2jdnIw.png)The path highlighted in yellow is for POST requests

When the API server receives a POST request, it sends that request to a queue, in our case AWS SQS. Later, on a DBWriter server, the execution path is summarized as:

**🟢Dequeue**→🔴**Mutate**→🟢**Notify**

## Checklist x Execution Path <a href="#b226" id="b226"></a>

![](https://miro.medium.com/freeze/max/30/1\*XzI5gJ0i6KzM5DiU0HVuRw.gif?q=20)![](https://miro.medium.com/max/315/1\*XzI5gJ0i6KzM5DiU0HVuRw.gif)Writing endpoints should be

Let’s now connect our checklist with the two execution paths for a POST request.

> Circles 🟢🔴🔵 come from our execution path; beneath each we identify which parts of our ✅ checklist they address.

### **Path 1: Request Receipt** <a href="#abba" id="abba"></a>

On the API server

1. 🔵 **Receive:** Route the request to the correct **Controller**

* RESTful routing ✅
* Control flow ✅

2\. 🟢 **Validate:** Extract the input parameters, validate their syntax, and authenticate the profile in the **Request,** using **Params** and **Validators.** Return the inputs required to perform the operation at the service layer.

* Parameter Extraction and Coercion ✅
* Authentication ✅
* Syntactic validation ✅

3\. 🔴 **Validate:** Validate the inputs against business rules using a **Service**

* Semantic Validation ✅
* Authorization ✅

4\. 🟢 **Enqueue:** Configure the **JobConfig** and enqueue the job.

* Job Configuration ✅
* Job Enqueuing ✅

5\. 🟢 **Respond:** Hand back receipt confirmation, with a 200 OK status code in the **Response**, that the write operation has been successfully enqueued and the optional JSON data.

* Presentation ✅

### **Path 2: Request processing** <a href="#c2d5" id="c2d5"></a>

On the DBWriter server

1. 🟢 **Dequeue:** Receive enqueued job with **Job** or **Service**

* Queue Processing ✅

2\. 🔴**Mutate**: Change the database via operations defined in the Service

* State mutation ✅
* Logging ✅

3\. 🟢 **Notify:** Send back an asynchronous Socket Message to the client that the operation has succeeded or failed.

* Async Relay ✅

## Next up <a href="#1e26" id="1e26"></a>

In the section above we defined a checklist of operations that all endpoints must fulfill. We then connected that checklist to the execution paths suggested by our Clean API architecture. Now we are going to see what this looks like in practice.
