# Performance Practise for API

## &#x20;Performance Best Practices for REST APIs <a href="#26ce" id="26ce"></a>

[![Abdul Wahab](https://miro.medium.com/fit/c/62/62/1\*ynM5Vl9kQoywa1Zij3QU8w.jpeg)](https://abdulrwahab.medium.com/?source=post\_page-----1d4a5922dae1-----------------------------------)![](https://miro.medium.com/max/1540/1\*OQhrUIqYSB6xVtXIeLOcGA.jpeg)Image Source: Wall Street Journal (wsj.com)

In my [**previous segment**](https://abdulrwahab.medium.com/api-architecture-best-practices-for-designing-rest-apis-bf907025f5f), I shared some best practices on how to _design_ effective REST APIs.

A well-thought out design must also take into account the **performance** aspects of an API. Good design means little if the API _does not perform_ as desired in response to increasing requests, and evolving business and/or customer requirements.

## So… what is API Performance? <a href="#5e5e" id="5e5e"></a>

![](https://miro.medium.com/freeze/max/66/1\*aViqX3Qh0pdJrcMoxQdSNg.gif?q=20)![](https://miro.medium.com/max/812/1\*aViqX3Qh0pdJrcMoxQdSNg.gif)Image Source: giphy

Just like any performance, API performance is largely about how an API responds and functions, _in response to different types of requests_ it receives.

_Example:_ Let’s say we have a customer-facing app that displays a customer’s current order. Our app fetches the order details from an API. But now, customers have indicated that they want to view _all of their orders_ (past and current) in _one place_. So, we build a _My Orders_ page for displaying all orders for the customer. Which means, our API will now be returning _more data_, and larger payloads than it was returning before.

How do we ensure that our API is able to return all of the data to present back to our customer, without issues like: _latency, server-side errors, and excessive requests?_

## Performance Enhancement Tips <a href="#3318" id="3318"></a>

### 1- Reduce and limit the Payload Size <a href="#8e70" id="8e70"></a>

![](https://miro.medium.com/freeze/max/66/1\*dK6mJgHMGEkK1msB7PWfMw.gif?q=20)![](https://miro.medium.com/max/1056/1\*dK6mJgHMGEkK1msB7PWfMw.gif)Image Source: giphy

Extremely large payloads of response data will _slow down_ request completion, other service calls, and in affect degrade performance. As you know, now that we are returning _all orders_ for the customer as opposed to just their current order, we will have to deal with some performance degredation.

We can use [GZip Compression](https://developers.google.com/blogger/docs/3.0/performance#gzip) to reduce our payload size.

This lessens the download size of our response on the web app (client side), as well as increase the upload speed, or creation of some entity (orders).

We can use `Deflate compression` on a Web API.

Or, we can update the `Accept-Encoding`request header to `gzip` .

### 2- Enable caching <a href="#e46a" id="e46a"></a>

![](https://miro.medium.com/freeze/max/66/1\*tSS8-LWKwGWPIQUuw3nAQQ.gif?q=20)![](https://miro.medium.com/max/484/1\*tSS8-LWKwGWPIQUuw3nAQQ.gif)Image Source: Tenor

Caching is one of the easiest methods to improve an API’s performance. If we have requests that frequently give back the _same response_, then a cached version of that response helps avoid additional service calls/database queries.

You will want to make sure when using caching to periodically invalidate the data in the cache, especially when new data updates occur.

_Example:_ Let’s say our customer wants to place an order for an auto part, and our app calls out to some Auto Parts API to fetch the part price. Since the response (part price) only changes once every week (@ 12:00am), we can cache the response for the rest of the time until then. This saves us from making a new call everytime to return the same part price.

### 3- Provide sufficient Network speed <a href="#ee6d" id="ee6d"></a>

![](https://miro.medium.com/freeze/max/66/1\*2iHp0AkNWWOHsZBtUuTHsA.gif?q=20)![](https://miro.medium.com/max/1096/1\*2iHp0AkNWWOHsZBtUuTHsA.gif)Image Source: Tenor

A slow network will degrade the performance of even the most robustly-designed API. Unreliable networks can cause downtime, which could cause your organization to be in violation of SLAs, terms of service, and promises you may have made to your customers. It is important to invest in the proper network infrastructure, so that we can maintain the desired level of performance.

This can be achieved by leveraging and purchasing sufficient cloud resources and infrastructure (example: in AWS, allocate the proper # of EC2 instances, use a Network Load Balancer).

Also, if you have a large amount of background processes, run those on separate threads to avoid blocking requests. You can also use mirrors, and Content Delivery Networks (CDNs) such as CloudFront to serve requests faster around different parts of the globe.

### 4- Prevent accidental calls, slowdowns, and abuse <a href="#f5e1" id="f5e1"></a>

![](https://miro.medium.com/freeze/max/66/1\*NZyKj4WB1tFmQx4D6NZGCw.gif?q=20)![](https://miro.medium.com/max/484/1\*NZyKj4WB1tFmQx4D6NZGCw.gif)Image Source: Tenor

You can have a situation where your API suffers a DDoS attack that can either be malicious and intentional, or unintenional when an engineer calls the API to execute on a loop from some local application.

You can avoid this by _measuring transactions_, and monitoring the number of how many transactions occur per IP Address, or per SSO/JWT Token (_if the customer/calling app is authorized before calling the API_).

This method to **rate-limiting** helps reduce excessive requests that would slow the API down, helps deal with accidental calls/executions, and proactively monitor and identify possible malicious activity.

### 5- Try to use PATCH over PUT <a href="#f499" id="f499"></a>

![](https://miro.medium.com/freeze/max/66/1\*YQ-KYd2JSGzacYzgpnOsOg.gif?q=20)![](https://miro.medium.com/max/484/1\*YQ-KYd2JSGzacYzgpnOsOg.gif)Image Source: Tenor

It is a common misconception among engineers that `PUT` and `PATCH` operations yield the same result.

They are _similar_ in updating resources, but they each perform the updates _differently._

`PUT` operations update resources by sending updates to the _entire resource_. `PATCH` operations apply partial updates to only the resources that need updating. Resulting in`PATCH` calls that produce smaller payloads, and improve performance at scale.

**💡Pro-Tip:** Even though `PATCH` calls can limit the request size, you should note that it is not Idempotent. Meaning, _it is possible_ that a `PATCH`can yield different results with a series of multiple calls. So, you should _carefully and deliberately_ consider your application for using `PATCH` requests, and make sure that they are idempotently implemented if needed. If not, use `PUT` requests.

![](https://miro.medium.com/max/66/1\*b6VfOSSDLBK\_S-IGor9q1w.png?q=20)![](https://miro.medium.com/max/1540/1\*b6VfOSSDLBK\_S-IGor9q1w.png)Image Source: nordicapis.com

### 6- Enable Logging, Monitoring, and Alerting <a href="#1460" id="1460"></a>

![](https://miro.medium.com/freeze/max/66/1\*KxxcIZDEBx-JSQgQQmcgHg.gif?q=20)![](https://miro.medium.com/max/1540/1\*KxxcIZDEBx-JSQgQQmcgHg.gif)Image Source: hellodewww — Wordpress.com

This is perhaps one of _**the most important tips**_ you will read here. If there is one thing you should learn from this article, it should be this! **No negotiation** on this one.

Having logs, monitoring, and alerting help engineers diagnose and remediate issues _ **before they become problems**_. Many APIs (Express/Node-based, Java, Go) have predefined endpoints for assessing things like:

* `/health`
* `/metrics`

If you do not have logging enabled, and there is a potential issue going on, you will not be able to track the origin, or where the problem is occurring in a particular request.

If you do not have monitoring enabled, you will not know from an analytical perspective how often some problems or errors are occurring. Which will then prevent you from thinking of possible solutions.

And… if you do not have alerting enabled, you will not know whether there is a problem, UNTIL a customer (or worse, customer_**s**_) report it. **SCARY**!

### 7- Enable Pagination <a href="#e454" id="e454"></a>

![](https://miro.medium.com/freeze/max/66/1\*Aav3i3nzhu6pdtc8142LmA.gif?q=20)![](https://miro.medium.com/max/1540/1\*Aav3i3nzhu6pdtc8142LmA.gif)Image Source: UX Design World

Pagination helps create buckets of content from multiple responses. This sort of optimization helps improve responses while preserving the data that is transferred/displayed to a customer.

You can standardize, segment, and _limit the responses_ which help reduce complexity of results, and improve the overall customer experience by providing the response/results _only for what a customer has asked for_.

### Closing thoughts <a href="#a495" id="a495"></a>

![](https://miro.medium.com/freeze/max/66/1\*EOrT\_fmP\_XTB9--0Us3oSw.gif?q=20)![](https://miro.medium.com/max/1056/1\*EOrT\_fmP\_XTB9--0Us3oSw.gif)Image Source: Giphy

We know that APIs are amazing, and can be extremely powerful in providing the business and customers a great experience, _if_ properly optimized and enhanced for performance.

Business requirements and customer expectations _**always evolve over time**_. And as responsible engineers, it is up to us in deciding how to build our APIs in a performant manner, that can help us achieve and exceed our goals.

These tips are just the tip of the iceberg, and apply to all APIs in a general setting. Depending on your particular API and use case, what services it interacts with, how often it gets called, from where it gets called, etc. you might have to implement these tips in different ways.
