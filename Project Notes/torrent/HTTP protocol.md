## HTTP 

**Port: 80**

HTTP stands for Hypertext Transfer Protocol. It is a set of rule which is used for transferring data like, audio, video, images, text and other multimedia files on world wide web. HTTP does not have any security.

<br>

**HTTP 1.0**

1. HTTP 1.0 was designed to use a new TCP connection for every request.
2. Slow
3. High Latency

**HTTP 1.1**

1. Version 1.1 solved above problem with the use of persistent connections and pipelining requests on a persistent connection.
2. Low Latency
3. Streaming with chunked transfer (For example loading image part by part)

<br>

**HTTP Request Methods**

1. GET - This method retrieves information from the given server using a given URI
2. POST - The POST request sends the data to the server. For example, file upload, customer information, etc. using the HTML forms.
3. PUT - It is used to replace all the current representations of the target resource with the uploaded content.
4. DELETE - It is used to remove all the current representations of the target resource, which is given by URI.
5. CONNECT - It is used to establish a tunnel to the server, which is identified by a given URI.

<br>

## HTTPs

**Port: 443**

HTTPS stands for Hyper Text Transfer Protocol Secure. Https transfers data in the encrypted format. Thus, https prevents hackers from reading and modifying the data during the transfer between the browser and web server. HTTPS established an encrypted link between the browser and the web server using the Secure Socket Layer (SSL) or Transport Layer Security (TLS) protocols. 

**Secure Socket Layer (SSL)**

SSL ensures that the data transfer between the two systems remains encrypted and private. SSL establishes an encrypted link using an SSL certificate which is also known as a digital certificate. 

**Transport Layer Security (TLS)**

TLS is the evolved version of SSL. HTTPS is an implementation of TLS encryption on top of the HTTP protocol. Any website that uses HTTPS is therefore employing TLS encryption.

<br>

![img](https://github.com/naman14310/Interview_Prep/blob/main/Project%20Notes/torrent/images/TLS%201.2.jpeg)

In TLS 1.2 we encrypt the data using some key at client side and same key will also be needed to decrypt that data. Now we before sending the encrypted data, we need to send the key to the server side. So we encrypt the key with server's public key (which can only be decrypted by server's private key as per assymmetric key cryptography). And then it transmit data to the server.

In TLS 1.3, Diffie Helman Key exchange algorithm is used to provide more security.

<br>

-----

<br>

## Interview Questions

#### 1. What is the Status Code?

HTTP response status codes indicate whether a specific HTTP request has been successfully completed. Responses are grouped in five classes:
1. Informational responses (100–199)
2. Successful responses (200–299)
3. Redirects (300–399)
4. Client errors (400–499)
5. Server errors (500–599)

<br>

#### 2. What are Persistent Connections?

In HTTP/1.0, the connection is closed after a single request or response pair. In HTTP/1.1, a mechanism was introduced, which is known as keep-alive-mechanism. In this mechanism, a connection could be reused for more than one request.

<br>

#### 3. What is Session State in HTTP?

Session state is also known as Stateless state. HTTP is a stateless protocol. In the session state, the client and server just know about each other only during the current request. If the connection is closed, and two computers want to connect again, they need to provide information to each other as a new connection, and the connection is handled as the very first one.

<br>

#### 4. What is HTTP Short polling ?

A lot of requests are processed as they come to the server, creating a lot of traffic. Below is an example of short polling.
 
```
00:00:00 C-> Is the cake ready? 
00:00:01 S-> No, wait.
00:00:01 C-> Is the cake ready?
00:00:02 S-> No, wait.
00:00:02 C-> Is the cake ready? 
00:00:03 S-> Yeah. Have some lad.
00:00:03 C-> Is the other cake ready?
```

<br>

#### 5. What is HTTP Long polling ?

With HTTP long-polling, the client continually polls the server requesting for new information. The server hold the request open until any new data is available, and responds with the new information once available. In essence, the client is constantly polling the server to see if the database has changed state. As soon as the client receives that new information, it can immediately send another request and the operation is continually repeated.

<br>

#### 6. What is Periodic polling ?

There’s a predefined time gap between two requests. This is an improved/managed version of polling. You can reduce server consumption by increasing the time gap between two requests. But if you need to be notified with no delay when the server event happens, this is not a good option.

<br>

#### 7. What is URI?

A Uniform Resource Identifier (URI) is a type of identifier that is used to identify either the source or destination of hypertext data.

<br>

#### 8. What are Idempotent methods?

If an identical request is made multiple times and the results have been the same, then this method is called Idempotent. And the PUT and DELETE methods are known as Idempotent Methods.

