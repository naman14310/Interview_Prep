# All about Proxies

<br>

### What’s a proxy server?
A proxy server, or web proxy, is a server that sits in front of a group of client machines. When those computers make requests to sites and services on the Internet, the proxy server intercepts those requests and then communicates with web servers on behalf of those clients, like a middleman.

![img](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/forward-proxy-flow.svg)

<br>

**Reasons to use a proxy server:**
1. To achieve anonymity and protect our identity online.
2. Fetching static pages with low latency using Caching.
3. Block a group of users from accessing certain sites.
4. To avoid state or institutional browsing restrictions.

<br>

### What's a Reverse Proxy?
A reverse proxy is a server that sits in front of web servers and forwards client (e.g. web browser) requests to those web servers. Reverse proxies are typically implemented to help increase security, performance, and reliability.

![img](https://www.cloudflare.com/img/learning/cdn/glossary/reverse-proxy/reverse-proxy-flow.svg)

<br>

**Benefits of using reverse proxy:**
1. Load Balancing - distributes the incoming traffic evenly among the different servers to prevent any single server from becoming overloaded. 
2. Caching - results in faster response time.
3. Protection from attacks, such as a DDoS attack.
4. Global Server Load Balancing (GSLB) - In this form of load balancing, a website can be distributed on several servers around the globe and the reverse proxy will send clients to the server that’s geographically closest to them. 
5. Encrypting and decrypting SSL (or TLS) communications for each client can be computationally expensive for an origin server. A reverse proxy can be configured to decrypt all incoming requests and encrypt all outgoing responses, freeing up valuable resources on the origin server.


<br>

**How reverse Proxy is different from forward proxy?**

A forward proxy sits in front of a client and ensures that no origin server ever communicates directly with that specific client. On the other hand, a reverse proxy sits in front of an origin server and ensures that no client ever communicates directly with that origin server.
