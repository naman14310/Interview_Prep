## HTTP 

**Port: 80**

HTTP stands for Hypertext Transfer Protocol. It is a set of rule which is used for transferring data like, audio, video, images, text and other multimedia files on world wide web. HTTP does not have any security.

HTTP Request Fields: URL, Method Type, Headers, Body

HTTP Response Fields: Status Code, Headers, Body

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

In TLS 1.3, Diffie Helman Key exchange algorithm is used to provide security while exchanging the key between client and server.
