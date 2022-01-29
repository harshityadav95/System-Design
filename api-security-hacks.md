# API Security Hacks

Source :[https://labs.detectify.com/2021/08/10/how-to-hack-apis-in-2021/](https://labs.detectify.com/2021/08/10/how-to-hack-apis-in-2021/)



## How to Hack APIs in 2021

August 10, 2021

[_**Detectify Crowdsource**_](https://detectify.com/crowdsource/ethical-hacking-with-crowdsource) _**is not your average bug bounty platform. It’s an invite-only community of the best ethical hackers who are passionate about securing modern technologies and end users. Crowdsource hackers**_ [_**Hakluke**_](https://www.twitter.com/hakluke) _**and**_ [_**Farah Hawa**_](https://twitter.com/farah\_hawaa) _**have joined forces on this guest blog on how hackers and defenders can (safely) hack APIs to help make the Internet safer.**_

Baaackkk iiin myyy dayyyyy APIs were not nearly as common as they are now. This is due to the explosion in the popularity of Single Page Applications (SPAs). 10 years ago, web applications tended to follow a pattern where most of the application was generated on the server-side before being presented to the user. Any data that was needed would be gathered directly from a database by the same server that generates the UI. It might look something like this:

![most of the application was generated on the server-side before being presented to the user](https://labs.detectify.com/wp-content/uploads/2021/08/same-server.png)

_Chart: Web application model 10 years ago_

Many modern web applications tend to follow a different model often referred to as an SPA (Single Page Application). In this model there is typically an API backend, a JavaScript UI, and database. The API simply serves as an interface between the webapp and the database. All requests to the API are made directly from the web browser.

[_Detectify product update: the fuzzing engine will cover public-facing APIs_](https://blog.detectify.com/2021/08/03/detectify-fuzzing-public-facing-apis/)

![All requests to the API are made directly from the web browser.](https://labs.detectify.com/wp-content/uploads/2021/08/API.png)

_Chart: Modern web applications tend to follow a different model often referred to as an SPA_

This is often a better solution because it is easier to scale and allows more specialised developers to work on the project, i.e. frontend developers can work on the frontend while backend developers work on the API. These apps also tend to feel snappier because page loads are not required for every request.

Instead, different components of the same page will update ✨magically✨, giving it a similar feel to a native application. This model has also become more popular because ten billion ⁽ᶜᶦᵗᵃᵗᶦᵒⁿ ⁿᵉᵉᵈᵉᵈ⁾ different frontend JavaScript frameworks (React, Vue and Angular, etc.) have come into existence. Suspicious minded folk might conclude that the ridiculous amount of JavaScript frameworks available today is a co-ordinated attempt to slow the progress of webapp development, instigated by the Illuminati. That’s probably not true though 👀.

All this to say – there are APIs everywhere now, so we should know how to hack and secure them. If you’re still reading – your fingers are probably hovering over ctrl+w. Your brain is thinking “this article title promised to teach me to hack, not what a SPA is. I am an intellectual individual and the author’s attempts at humour are futile, life is short and I am wasting my time reading this stupi….” HOLD IT! We’re getting there. I promise. Cool your jets. [Goooooosfraba](https://www.youtube.com/watch?v=rWIcubkPaxo).

### Setting up for testing APIs

Postman is a handy application that makes API security testing a breeze. You can download Postman from its [official website](https://www.postman.com/downloads/). In essence, Postman is just another HTTP client which can be used to easily modify and send requests to APIs.

If you’ve got a collection of API requests in a file, you can start by importing them into Postman by clicking on the **Import** button on the top-left corner of the app:

![postman API calls](https://labs.detectify.com/wp-content/uploads/2021/08/postman-API-calls-Detectify-Labs.png)

After importing the collection, you will see the API calls loaded in Postman like so:&#x20;

![API calls in Postman](https://labs.detectify.com/wp-content/uploads/2021/08/unnamed-3.png)

By clicking on an individual API call, the full API request will be seen on the right. Moreover, different parts of the request will be broken down into sections like **Params, Authorization, Headers, Request Body**, etc. This will allow you to easily play around with each part.

![parameters of API call requests in postman](https://labs.detectify.com/wp-content/uploads/2021/08/parameters-of-API-call-requests.png)

You can modify the request headers and body in the same manner as you would in BurpSuite. To analyze the responses to your test cases, you can simply hit the **Send** button on the top right.

![how to modify in postman](https://labs.detectify.com/wp-content/uploads/2021/08/how-to-modify-in-postman.png)

The response view in Postman looks like this and the response is also bifurcated into different sections like Body, Cookies, Headers, etc. so you can analyze each part carefully.

![response view in postman](https://labs.detectify.com/wp-content/uploads/2021/08/response-view-in-postman.png)

Since Postman is meant for APIs specifically, you have a lot of in-built options to test for functions that are mostly present in APIs. For example, by clicking on this drop-down arrow next to the request method, you can see tons of different request verbs to test with:

![GET in postman](https://labs.detectify.com/wp-content/uploads/2021/08/GET-in-postman-169x300.png)

For GET requests, you can add/remove as well as edit parameters via the **Params** tab. When you check/uncheck parameters, you can see them appear accordingly in the URI field.

![get postman 2](https://labs.detectify.com/wp-content/uploads/2021/08/get-postman-2.png)

When it comes to adding Authorization values, Postman gives you a number of options to choose from and you can choose one depending on how the target API handles authorization.

![authorization of values in postman](https://labs.detectify.com/wp-content/uploads/2021/08/authorization-of-values.png) ![](https://labs.detectify.com/wp-content/uploads/2021/08/authorization-of-values-crop.png)

You can also add Authorization values to the entire collection or sub-collection by following these steps:

1. Click the three dots on the side of the collection/sub-collection name and choose the **Edit** option\
   ![authorization step 1](https://labs.detectify.com/wp-content/uploads/2021/08/authorization-step-1.png)
2. Go to the **Authorization** tab, select the type of auth and add its value.\
   ![authorization step 2](https://labs.detectify.com/wp-content/uploads/2021/08/authorization-step-2.png)\

3. Lastly, go to an individual API request and select the **Inherit auth from parent** option\
   ![authorization step 3](https://labs.detectify.com/wp-content/uploads/2021/08/authorization-step-3.png)\


This will save you the trouble of setting Authorization values for every single request.

Another handy feature of Postman is that it allows users to proxy API requests with BurpSuite. In order to set that up, you need to follow these steps:

1. Click on the Settings option from the drop-down menu on the top-right corner\
   ![settings](https://labs.detectify.com/wp-content/uploads/2021/08/settings-300x144.png)
2. Go to the **Proxy** tab and do this:
   * Switch Off **Use the system proxy**
   * Switch On **Add a custom proxy configuration**
   * Set the **Proxy Server IP address** & **port** to match your Burp Suite proxy settings. The default values are 127.0.0.1 and 8080.\
     Your final settings should look like this:\
     ![Final settings](https://labs.detectify.com/wp-content/uploads/2021/08/final-setting-.png)\

3. To proxy HTTPS requests without any errors, you can switch off **SSL certificate validation** under the **General** tab.\
   ![toggle ssl certs](https://labs.detectify.com/wp-content/uploads/2021/08/toggle-SSL-certs-300x212.png)

[_Detectify product update: the fuzzing engine will cover public-facing APIs_](https://blog.detectify.com/2021/08/03/detectify-fuzzing-public-facing-apis/)

### Types of API Vulnerabilities

APIs come in many shapes and sizes, the methods of attacking an API will vary greatly depending on these shapes, and sizes. It would be impossible to cover every attack type in a single blog, but we’re going to go through a bunch of them!

#### API Exposure

Much like web applications, APIs can have different levels of visibility. Some may be accessible to the internet while others are only available internally. One of the more rudimentary API hacks is simply gaining access to an API which should be inaccessible to you. This may be achieved through a variety of methods, including:

* **Forced browsing:** If you are lucky, an API that is intended for internal use may be accidentally exposed to the internet, either through a misconfiguration or just because it was assumed that nobody would be able to find it. API locations may be discovered through many means including analysing JavaScript files, analysing exposed source code, observing host names (e.g. [api.internal.example.com](http://api.internal.example.com)) and Google dorking.
* **Pivoting:** Discovering an exploit like SSRF on an external host may allow you to pivot into an internal API.

**Mitigation**

There are many best practices that can help mitigate against unintentionally exposing APIs including the implementation of strict deployment practices, enforced principle of least privilege through IAM and network segmentation.

#### Misconfigured Caching

For APIs that require authentication, the data being returned is often dynamic and is scoped to each API key. For example, accessing `/api/v1/userdetails` as Bob should return Bob’s details, while accessing the same endpoint as Jane should return Jane’s details.

A common misconfiguration occurs when an API does not use the standard `Authorization` header, instead using a custom header such as `X-API-Key`. Caching servers may not recognise this as an authenticated request, and may cache it.

If this is the case, and there are no `Cache-Control` or `Pragma` headers, simply accessing `/api/v1/userdetails` may reveal the information of another user.

**Mitigation**

The fix for this is to implement `Cache-Control` or `Pragma` headers and utilise the standard `Authorization` header.

#### Exposed tokens

We shouldn’t discount the most rudimentary authentication hack. Discovering an API key through any means may provide you with access to the API. To make matters worse, APIs that are meant for internal use often have no need to implement complex authentication flows and thus may implement a static token as their authentication. Secret tokens may be discovered in code repositories, client-side JavaScript, intercepting traffic, etc.

**Mitigation**

Implementing code scanning into your devops pipeline will often catch API keys before they are deployed somewhere they shouldn’t be. Some code repository providers (including [GitHub](https://docs.github.com/en/code-security/secret-security/about-secret-scanning)) also have the ability to detect API keys before they are pushed.

#### JWT Weaknesses

If your API token is three base64 blobs separated by two dots (.), it’s probably a [JSON Web Token (JWT)](https://jwt.io). Like many things, these tokens are secure in theory, but there are many ways to mess up the implementation in a way that introduces security issues. Before we delve into JWT attacks we’ll go through a very quick JWT primer.&#x20;

Here’s an example JWT token, colorized for your aesthetic appreciation:

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV\_adQssw5c

There are three base64 encoded strings separated by dots, the first section (colored in red) is the `header`. The second (colored in purple) is the `payload`, and the third (colored in blue) is the `signature`.

If we decode the first section, also known as the header, we will see the following:

`{`\
`"alg": "HS256",`\
`"typ": "JWT"`\
`}`

This outlines the algorithm that we’re using (HS256) and the type of token (JWT). If you’ve not worked with JWTs before you might be thinking _“why do we need an algorithm?”_. We will get there soon!

If we decode the second section, also known as the payload, we will see the following:

`{`\
`"sub": "1234567890",`\
`"name": "John Doe",`\
`"iat": 1516239022`\
`}`

This section could contain anything, but at minimum it needs to contain some kind of user identifier and a timeout (iat).

The third section (also known as the signature) signs the first two sections with a secret key. In this case it is signed with the HS256 algorithm, which can be determined by looking at the “alg” value in the header. The idea is that the secret key should only be known to the owner of the application. When the application receives a JWT token, it can verify that the token is legitimate by decrypting the signature and comparing it to the data in the header and payload. If the data matches up then the data is verified, otherwise it is invalid.

So why would anyone use JWT? It is because it negates the need for server-side session management. Traditionally when a user logs in, an application would assign a secret token to the user and store that same token in a database. Whenever the user makes a request with their token, the application needs to check if the token is in the database. If it is, the user is allowed to continue, otherwise they are not. When we use JWTs, we introduce a method of trusting data that is sent from the client instead of solely trusting information that is stored in the database. If the application receives a JWT token that can be verified using the secret key, then the application has no reason to distrust it.

As we touched on earlier, in theory JWT tokens are totally secure. The problem is that they are often implemented in a way that is insecure. Here are some examples:

* The None algorithm: Some implementations of JWT will allow you to specify “None” as the algorithm. If the algorithm is “None”, the application will not check the validity with the signature, so you can simply update the payload to whatever you want. The most obvious exploitation of this would be to update the user id to another user to take control of their account.
* Brute forcing: It is possible to brute force the secret key of JWT tokens. The feasibility of this attack will depend on the strength of the key. You can attempt to crack JWT tokens using [this tool](https://github.com/brendan-rius/c-jwt-cracker). A full write-up on the method can be found on [Auth0’s blog](https://auth0.com/blog/brute-forcing-hs256-is-possible-the-importance-of-using-strong-keys-to-sign-jwts/).
* Simply changing the payload: In some rare cases, the server may simply skip the token verification entirely and trust the data in the payload. While I have not seen this personally, I have read about it occuring in the wild!
* Switching RS to HS: There’s a defect with some older JWT libraries where you can trick an application that is expecting tokens signed using asymmetric cryptography into accepting a symmetrically signed token. The symmetrically signed token that ends up being used is actually a public key which is often somehow obtainable, or reused from their HTTPS key. There is a great writeup of this method [here](https://www.nccgroup.com/ae/about-us/newsroom-and-events/blogs/2019/january/jwt-attack-walk-through/).
* The `iat` timeout is not honoured, so JWT tokens remain valid forever.

**Mitigation**

The best mitigation for JWT weaknesses is to utilise a widely-used, reputable JWT library for all JWT operations.

#### Authorization Issues / IDOR

Authorization is the process of checking whether an authenticated user has access to a specific user. A common authorization-related vulnerability is known as an Insecure Direct Object Reference (IDOR). For example, in an API for an invoicing application, we may have an endpoint that is used to get the details of an invoice:\
\
`/api/v1/invoices/?id=1234`

The `id` parameter is the identifier for the invoice that should be returned. If this endpoint is secured, I should only be able to get the details of invoices that belong to me. For example if I created an invoice with an ID of 1234 then it should return the details. If I try to access an invoice that I did not create by browsing to `/api/v1/invoices/?id=1233`, it should return an error.

If I am able to change the identifier to view the invoice details of other users, this is a vulnerability known as an IDOR.

To counter IDOR issues, many APIs today are utilising UUIDs as object identifiers. A UUID looks like this:

`f1af4910-e82f-11eb-beb2-0242ac130002`

It’s important to note that utilising UUIDs as identifiers is not a valid method of mitigating IDOR issues. In fact, the [UUID RFC](https://datatracker.ietf.org/doc/html/rfc4122#section-6) specifically calls this out:

> Do not assume that UUIDs are hard to guess; they should not be used as security capabilities (identifiers whose mere possession grants access), for example.  A predictable random number source will exacerbate the situation.

While it is good practice to utilise UUIDs as object IDs instead of integers, they should never be used as the sole protection against IDOR attacks.

**Mitigation**

Authorization issues are typically difficult to detect in an automated fashion. The structure of the codebase should be set up in a way that it is difficult to make authorization errors on specific endpoints. To achieve this, authorization measures should be implemented as far up the stack as possible. Potentially at a class level, or using middleware.

#### Undocumented Endpoints

It is common to encounter situations where the API you are attacking has no documentation (or at least none that is accessible to you). It is also fairly common that an API with documentation will have endpoints beyond what is documented. Sometimes the very existence of these endpoints may be a security issue – for example, the endpoint may be designed for administrative purposes, and allow you to perform administrative tasks as an underprivileged user. Other times, these endpoints may present vulnerabilities simply because they have not been tested as thoroughly as the ones that are easy to discover.

Here are a few methods that we can utilise to uncover undocumented endpoints:

* Use an application that interfaces with the API, and capture traffic. Perhaps the most commonly used tool for this today is Burp Suite.&#x20;
  * Set up Burp Suite to proxy the application traffic
  * Use all the application features
  * Check the endpoints that were used by viewing the Target tab.
* Observe errors. Many APIs will give errors that are verbose enough to enumerate undocumented endpoints and parameters. For example, sending a blank POST request to `/api/v1/randomstring` may result in an error that says something along the lines of `Invalid route, valid routes are [/users,/invoices,/customers]`.
* Analyse client-side JavaScript. If you know of an application that interacts with the API, you can analyse the JavaScript within that application to gather a list of API endpoints that may be accessible.
* Use [Kiterunner](https://github.com/assetnote/kiterunner), a tool by Assetnote that is designed for content discovery on APIs
* Brute force endpoints, there are some **excellent** API wordlists on [Assetnote’s website](https://wordlists.assetnote.io)

**Mitigation**

Having a strong, structured method and process for documenting API functionality can save a lot of headaches down the road. [Swagger](https://swagger.io/solutions/api-documentation/) is an excellent standard for exactly this. Additionally, it’s best to plan out the API functionality with documentation before coding it instead of the other way around.

#### Different Versions

When an organisation releases an API, it may interface with many different applications. If the API is updated at any point, it may introduce breaking changes for one or more of those applications. For that reason, multiple API versions are often implemented as a means of supporting older API schemas while also upgrading the API over time for new users.

It is worth testing all versions of the API. Older versions may still have security issues that have since been fixed in the new version, and newer / bleeding edge / beta versions may have introduced new security issues.

A common schema for api versioning is:

`/api/v1/`\
`/api/v2/`\
`/api/beta/`

It’s always worth checking for non-production route names such as:\
\
`qa`\
`devenv`\
`devenv1`\
`devenv2`\
`preprod`\
`pre-prod`\
`test`\
`testing`\
`staging`\
`stage`\
`dev`\
`development`\
`deploy`\
`slave`\
`master`\
`review`\
`prod`\
`uat`\
`prep`\
`Version2`\
``

This wordlist was taken from an [example set in DNSCewl](https://github.com/codingo/DNSCewl/blob/master/sets/non-production-hosts.txt).

**Mitigation**

API versions can be supported with defined lifecycles. For example, when you release version 2 of the API, you may notify your users that version 1 will reach End of Life (EoL) and be deprecated at a specific date in the future. Pre-production/beta versions should only be publicly accessible if they have been thoroughly tested for security issues.

#### Rate Limiting

Most of the time, APIs do not have any protection on the number of times a user can request them. This is termed as “lack of rate limiting” and it occurs when an attacker can call the API thousands of times to cause some unintended behaviour. The server will attempt to fulfill each of these requests and this can potentially:

* DOS the server by overloading it with requests
* Allow the attacker to quickly exfiltrate sensitive user information such as: user IDs, usernames, emails etc.
* Bypass authentication by brute-forcing a login API
* Flood the victim’s inbox by brute-forcing a functionality which sends an email/SMS to the victim

Let’s look at an attack scenario where there is no rate limiting on an API endpoint that checks credentials:

`GET /api/v1/user/1234/login/?password=mypassword`

Normally you would not see a password sent in a GET request like this, but for the purposes of this demonstration, let’s say that you do. To brute force the password in the endpoint above, an attacker can use [BurpSuite’s Intruder](https://portswigger.net/burp/documentation/desktop/tools/intruder) tool. This tool allows us to customize different kinds of brute-force attacks but for this example, we will be feeding it a simple list of passwords.

In the span of a few seconds, Intruder will make hundreds of API requests, attempting a different password on each request.

```
GET /api/v1/user/1234/login/?password=notmypassword1
GET /api/v1/user/1234/login/?password=notmypassword2
GET /api/v1/user/1234/login/?password=notmypassword3
.
.
.
GET /api/v1/user/1234/login/?password=correctpassword!
```

Since there is no rate limit, the server will happily serve the responses to all these requests and the attacker can continue to brute-force different passwords as quickly as possible until the correct one is found.

To protect against rate limiting bugs, the application should implement a limit on how often a user can request an API within a certain timeframe. The exact limit that is set will depend on the use case for that API or endpoint.

**Mitigation**

Rate-limiting can be implemented in many different ways. Per account, per IP address, per endpoint, for the entire API, etc. Some reverse proxies can also be used to implement application-wide throttling without additional development. The exact implementation that you use will depend on the requirements of your specific application.

#### Race Conditions

A race condition is when two or more requests are sent at the same millisecond to an API. When an API does not have a mechanism to handle this scenario, it can lead to the API processing the requests in an unintended manner.

A potential attack scenario for a race condition could arise while redeeming discounts or promo codes on a vulnerable e-commerce application.&#x20;

```
POST /api/v1/discount
Target: www.ecommerce.com
Connection: close

{
"code","first10",
"amount":"10"
}
```

BurpSuite has an extension called [Turbo Intruder](https://portswigger.net/bappstore/9abaa233088242e8be252cd4ff534988) which allows users to test for race conditions using the in-built `race.py` script.

After configuring the attack in Turbo Intruder, an attacker can send multiple concurrent requests to the API for redeeming this promo code.

```
POST /api/v1/discount		200 OK
POST /api/v1/discount		200 OK
POST /api/v1/discount		200 OK
POST /api/v1/discount		404 Not Found
POST /api/v1/discount		404 Not Found
```

If the API does not invalidate the promo code immediately after receiving the first concurrent request, the discount amount could be doubled, tripled, etc.

This attack is named “Race Condition” because it is a race between how fast an attacker sends requests to modify a resource and how fast the application updates that particular resource.

**Mitigation**

Unfortunately, mitigating race condition issues often means sacrificing performance. Methods to mitigate race conditions include utilizing locks and other thread-safe functionality. Most programming languages will have thread-safe functionality built in, but it usually needs to be manually specified.

#### XXE injection

XXE stands for [XML External Entity](https://blog.detectify.com/2018/04/17/owasp-top-10-xxe/) and this injection vulnerability can be tested anywhere an API is used to process XML data. SOAP APIs could also be vulnerable to XXE injection because they are based in XML.&#x20;

An API endpoint that uses XML looks something like this:

```
POST /soap/v2/user HTTP/1.1
Host: example.com
Content-Type: text/xml


 <?xml version="1.0" encoding="UTF-8"?><!DOCTYPE dtd[<!ENTITY username SYSTEM "https://example.com/?username">]>
<SOAP-ENV:Envelope>
<SOAP-ENV:Body>
<getUser>
<id>&username;</id>
</getUser>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

An XML document uses entities to represent a single object of data and an XML document type definition (DTD) is used to define the entities to be used, structure of the document, etc.

XXE injection refers to when an attacker injects custom external entities that are outside of the specified DTD. Once these external entities are parsed by the API, it can allow an attacker to access the application’s internal files, escalate to SSRF, leak sensitive data to an attacker-controlled domain or DOS the server.

This is an example of a request where an attacker has injected an external custom entity called xxe and the purpose of this entity is to retrieve an internal file:

```
POST /soap/v2/user HTTP/1.1
Host: example.com
Content-Type: text/xml

 <?xml version="1.0" encoding="UTF-8"?><!DOCTYPE aa [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<SOAP-ENV:Envelope>
<SOAP-ENV:Body>
<getUser>
<id>&xxe;</id>
</getUser>
</SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

If the API allows the usage of a standard XML parser to process the data, then this injected external entity will be processed by the application and will return the content of `/etc/passwd` to the attacker.

**Mitigation**

Ensure that the XML parser being utilised is set up to not parse XML entities.

#### Switching Content Type

Even though an API may use JSON as a data format to communicate, the underlying server/framework may still accept other data formats like XML. Therefore, when you see an API with a `content-Type` of `application/json`, you can still try testing for XXE by switching its value to `Content-Type: text/xml`

For example, if an API uses JSON:

```
POST /api/v1/user HTTP/1.1
Host: example.com
Content-Type: application/json
```

It can be modified to send XML data in this manner:

```
POST /soap/v2/user HTTP/1.1
Host: example.com
Content-Type: text/xml

 <?xml version="1.0" encoding="UTF-8"?><!DOCTYPE aa [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
```

**Mitigation**

This bug only really exists on frameworks that are designed to accept multiple formats, where each format corresponds to a type of object. In these cases, the frameworks would normally have an option to whitelist available formats. **It should be noted that this is quite rare in 2021.**

#### HTTP Methods

APIs usually support various types of HTTP methods. Some of the common ones are `GET`, `POST`, `PATCH`, `DELETE` and `OPTIONS`.If a GET request is sent at a point where an application expects a POST request, then this can lead to the application responding in unexpected ways.

Let’s take CSRF as an example. [CSRF (Cross-Site Request Forgery)](https://blog.detectify.com/2016/07/19/owasp-top-10-cross-site-request-forgery-8/) is when an attacker tricks the victim into submitting a request which can cause state-changing actions to occur on the victim’s account. These changes can be anything from changing the victim’s personal details and in some cases, even changing the victim’s password leading to a full account takeover.

A normal request to change the victim’s personal details may look something like this:

```
POST /api/v1/user
Target: www.example.com
Content-Type: application/json

{
  "name","victim", 
  "email":"victim@example.com", 
  "location":"AU", 
  "csrftoken":"76hhs683bki0"
}
```

After observing the above request, we might conclude that a CSRF attack will not be possible since the `csrftoken` value is being sent in the request body. However, if an API does not restrict the HTTP methods which can be used for this request, then it may be possible for an attacker to bypass this protection by sending this request using the GET method.

```
GET /api/v1/user?name=hacked&email=attacker@example.com&location=FR
Target: www.example.com
Connection: close
```

Additionally, sometimes the server simply does not validate the CSRF token, or accepts the request if no CSRF token parameter exists at all in the request.

Another common vulnerability in REST APIs is when permissions have been correctly applied on an endpoint, but only for one HTTP verb. For example, you may not be able to GET other people’s records, but you may be able to edit their records by utilizing the PATCH verb.

**Mitigation**

Manually specify HTTP verbs in your routes. Do not utilize verbs unnecessarily, and ensure that any endpoints that are specified do have the same controls applied against all verbs. Some frameworks will do this automatically, others will not.

#### Injection Vulnerabilities

Server-side injection flaws like SQL injection, RCE, command injection and [SSRF](https://blog.detectify.com/2019/01/10/what-is-server-side-request-forgery-ssrf/) can exist on APIs in the same way they exist on regular web applications. Whenever any injected data is directly passed to the interpreter on the backend, it leaves room for an attacker to send targeted queries and commands to access internal data or execute arbitrary code.

For example, let’s test SSRF on the following API request:

```
POST /api/v1/user
Target: www.example.com
Content-Type: application/json

{
  "name","victim", 
  "email":"victim@example.com", 
  "profile_pic_url":"https://www.example.com/me.jpg"
}
```

An attacker can attempt SSRF on the `website` field by sending a request like this:

```
POST /api/v1/user
Target: www.example.com
Content-Type: application/json

{
  "name","victim", 
  "email":"victim@example.com", 
  "profile_pic_url":"http://localhost/admin"
}
```

If the backend interpreter does not validate the `profile_pic_url` value properly, then this can lead to an attacker exploiting SSRF and successfully accessing internal data.

Similarly, an attacker can also attempt SQL injection if the data on this request is improperly validated:

```
POST /api/v1/orders
Target: www.example.com
Content-Type: application/json

{
  "offset":0,
  "limit":10,
  "scope":"orders_all",
}
```

By looking at the request body, we may get a hint that the values of these parameters are being sent to a backend database interpreter. Therefore, we can attempt SQL injection by modifying these values to targeted queries:

```
POST /api/v1/orders
Target: www.example.com
Content-Type: application/json

{
  "offset":0,
  "limit":10,
  "scope":"SELECT sleep(10)",
}
```

Once again, if the interpreter does not validate these values properly, then this can lead to a SQL injection where an attacker can cause a time delay in the database and even exfiltrate sensitive data.

**Mitigation**

Mitigating injection vulnerabilities in APIs is essentially the same as mitigating injection vulnerabilities in web applications. SAST/DAST scanners can help with this, but secure development practices are the best mitigation. Parameterize SQL queries, utilise ORMs and reputable libraries where possible, **don’t trust user input!**

**Written by:**\
Hakluke and Farah Hawa\
Detectify Crowdsource community members\
