# All About WebSockets

WebSockets is a technology that allows for opening an interactive communications session between a user’s browser and a server. With this technology, a user can send messages to a server and receive event-driven responses without requiring long-polling, i.e. without having to constantly check the server for a reply.

<br>

**Problem with HTTP Long Polling**

Long Polling involves making an HTTP request to a server and then holding the connection open to allow the server to respond at a later time when new data gets generated. Every time a user makes an HTTP request, a bunch of headers and cookie data are transferred to the server, which in turn increases latency. It is very expensive in terms of CPU, bandwidth consumption and storage.

**Websockets are the solution. How ?**

Websockets allow a long-held single TCP socket connection to be established between the client and server, allowing for the bi-directional, full duplex, messages to be instantly distributed. This is done with minimal overhead resulting in a low latency connection.

A request to a WebSocket connection is sent to the server from a client (or multiple clients) through a process called the WebSocket handshake, which starts with the client sending a regular HTTP request to the server. Part of this request includes an Upgrade header, which indicates to the server that the the client is trying to make a WebSocket connection. This request is called a WebSocket handshake.

This connection is maintained for each client, allowing data to be pushed in real-time back to the client from the server on demand. Because this connection between the server and client remains open, it eliminates the need for constant polling (i.e. checking) to see if new information has been stored on the backend. By doing so, this eliminates the need for constant fetch requests to the backend server.

<br>

**Applications of websockets**

The real-world applications for WebSockets are endless, including chatting apps, internet of things, online multiplayer games, and really just any real-time application.

<br>

**Advantages of WebSockets over HTTP long-polling**
1. Continuous connection between client and server.
2. Full duplex communication.
3. Very low latency. 
4. Limited HTTP overhead (like headers, cookies, etc.) making the speed at which data transfers happen much faster

<br>

**WebSockets Secure (wss)**

We strongly prefer the secure wss:// protocol over the insecure ws:// transport. Like HTTPS, WSS (WebSockets over SSL/TLS) is encrypted, thus protecting against man-in-the-middle attacks.

Note: wss connects on https only whereas ws connects on http
