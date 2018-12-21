# DeepTrace

DeepTrace is a distributed request tracking system for reactive HTTP-based micro-services. As such, it aims to share the responsibility of logging requests/responses with each application in a system.

That sounds great, right? But as always it's easer said than done.

Imagine you have the following chain of requests: a frontend SPA is making HTTP requests against your API which needs to request information from micro-service 1 which needs to collect information from micro-service 2.

Let's call this a **chain of requests**:

```txt
FRONTEND  ⇄ API ⇄ MS1 ⇄ MS2
```

How would you debug this chain of requests if at some point your API returns a 500 error?

Considering you can't control the client which is originating these requests, you'd need to log each request/response handled by your **applications** (reactive HTTP-based applications, such as APIs and micro-services). But how would you find which request/response from MS2 belongs to the request chain which resulted in your API returning an 500 error?

Can you imagine if you do need to debug an even more complex chain?

```txt
API ⇄ MS1 ⇄ MS2
    ⇄ MS3
    ⇄ MS4 ⇄ MS2
          ⇄ MS5 ⇄ MS6 ⇄ MS7
    ⇄ MS8 ⇄ MS9
```

Note that in this example MS2 will be requested twice within the same chain of requests what makes it even harder to debug.


## So, how can DeepTrace help me?

To be honest, DeepTrace isn't the perfect out-of-box plug-n-play solution you are looking for and it will probably requires changes to your existent code.

DeepTrace approaches this problem in a way that the responsibility for collecting good data is shared among **applications**. That means your application is responsible for reporting to DeepTrace's Tracing API information about each request/response handled - let's call it a **trace** - along with three keys used to index this trace which we'll call **context keys**:

- **Request id**: An unique id that should be assigned to each request handled by your application.

- **Parent id**: Whenever an application received a HTTP request within a chain of requests, the **Parent id** key holds the **Request id** of the request which originated it. This enables DeepTrace to establish relations between requests and follow them while debugging. Naturally the application's request which started a chain of requests won't have a **Parent id**. We'll just call it a **Root request**.

- **Chain id**: All traces within a chain of requests are indexed with the same **Chain id** in order to facilitate their identification. This **Chain id** is always the **Request id** of the **Root request** which originated the chain of requests.

Each HTTP request triggered by your application should propagate the **Parent id** and the **Chain id** key to the next application. That's how all DeepTrace Agents knows when the current request is part of a chain of request or is a **Root request**.

With traces indexed by these three keys, DeepTrace is able to follow relations and respond most concerning questions when debugging micro-services.


## Challenges

The first biggest challenge faced by DeepTrace lays with DeepTrace's Agents - which are responsible for collecting request/response information along with those three index keys - because each programming language and each framework might introduce difficulties on how to extract and how to propagate the **context keys**. We do not have enough man-power to cover every possible language/framework combination but we believe that by keeping the DeepTrace Tracing API simple it should be easy for anyone to contribute to this project by creating their own DeepTrace Agents.

The second - but not less important - biggest challenge is security. We opted to not include security layers within DeepTrace at all. I known this sounds harsh but I think we should solve one problem at a time. For now, **we recommend you to deploy DeepTrace on an isolated network or on a network protected by IP whitelisting**.
