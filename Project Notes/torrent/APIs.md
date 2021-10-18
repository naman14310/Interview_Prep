# Everything about API 

Application Programming Interface or API is a common software interface that allows two applications to communicate.

**Difference between API request and webpage request**

The major difference between an “API request” and a “webpage request” is what kind of data is provided in the response. A website returns HTML, CSS, and JavaScript which work together with your browser to render a webpage. Web APIs respond with data in a raw format, not intended to be rendered by a browser into a user experience. JSON and XML are the most common formats used for this raw data.


<br>

## REST APIs
REST is an architectural style, or design pattern, for APIs. REST stands for **REpresentational State Transfer**.

*When a RESTful API is called, the server will transfer to the client a representation of the state of the requested resource. The representation of the state can be in a JSON format.*

<br>

A resource can be any object the API can provide information about. In Instagram’s API, for example, a resource can be a user, a photo, a hashtag. Each resource has a unique identifier. 

Eg: when a developer calls Instagram API to fetch a specific user (the resource), the API will return the state of that user, including their name, the number of posts that user posted on Instagram so far, how many followers they have, and more.

<br>

Each Api call require following two things:
1. An identifier for the resource. This is the URL (Uniform Resource Locator) for the resource, also known as the endpoint.
2. The operation you want the server to perform on that resource, in the form of an HTTP method. The common HTTP methods are GET, POST, PUT, and DELETE.

Eg: fetching a specific Twitter user, using Twitter’s RESTful API, will require a URL that identify that user and the HTTP method GET.

<br>

### Why REST ?
1. REST is usefull because you're not tying your API to your client-side technology. It is accessible from a client-side Web project, an iOS app, an IoT device and even a Windows Phone. 
2. Another key idea in this architectural philosophy is that the server **supports caching and is stateless**. 
3. Caching is important, as if multiple requests for the same resource are requested, caching of the result of the request means that the scalability of the server should increase.
4. Likewise, statelessness is about making sure that calls to the API aren't tied to a particular server, which increases the likelihood of building scalable server infrastructures. Because the server is stateless, every call to it must include all the data required to execute the request. 

<br>

### What purposes is REST API used for ?
REST API is used where data demands are not continuous and stateless. For example, requesting live data when your application needs it or requesting historical timeseries data to display a chart. 

<br>

### When to use Websocket and when to use REST ?
REST is more versatile and can deliver almost all the available resources provided by us. It is not useful when data delivery needs to be super fast (like more than 100 requests a second) and continuous. REST is great for occasional communications.

Websockets are ideal in scenarios with higher loads. WebSockets are a great choice for handling long-lived bidirectional data streaming in a near real-time manner, Using WebSockets is a considerable investment, hence it is an overkill for occasional connections.

<br>

### What is API Gateway ?
1. It is single point of entry into modern Api enabled application. These can be monolithic or microservice based apps. 
2. The api gateway job is to protect critical Api traffic thus helping enterprises in increasing revenue, improve developer productivity and reduce downtime. 
3. It calls api endpoints based on different client requests and requirements. 
4. An api gateway enables you to maintain a single api domain with multiple endpoint versions. 

<br>

### WebHooks

If your servers need to communicate frequently and/or bidirectionally, go for WebSockets. If your servers communicate occasionally, use REST calls. If your servers need to communicate unidirectionally on an event trigger, then go for webhooks. 

<br>

### REST Constraints

In order for an API to be RESTful, it has to follow 5 constraints:

**1. Uniform interface**

Each request to the API contains all the information the server needs to perform the request, and each response the server returns contain all the information the client needs in order to understand the response.


**2. Client — server separation**

The client and the server act independently, each on its own, and the interaction between them is only in the form of requests, initiated by the client only, and responses, which the server send to the client only as a reaction to a request.


**3. Stateless**

Stateless means the server does not remember anything about the user who uses the API. It doesn’t remember if the user of the API already sent a GET request for the same resource in the past, it doesn’t remember which resources the user of the API requested before, and so on.

Each individual request contains all the information the server needs to perform the request and return a response, regardless of other requests made by the same API user.


**4. Layered system**

Between the client who requests a representation of a resource’s state, and the server who sends the response back, there might be a number of servers in the middle. These servers might provide a security layer, a caching layer, a load-balancing layer, or other functionality. Those layers should not affect the request or the response.


**5. Cacheable**

This means that the data the server sends contain information about whether or not the data is cacheable. If the data is cacheable, it might contain some sort of a version number. The version number is what makes caching possible: since the client knows which version of the data it already has (from a previous response), the client can avoid requesting the same data again and again. 



