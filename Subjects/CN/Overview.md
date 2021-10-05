# Basics of CN
A computer network is a set of computing devices connected through links or communication channels

<br>

## @ Some Basic Terminologies

### 1. Web Vs Internet
The Internet is a global network of networks while the Web, also referred as World Wide Web (www) is collection of information which is accessed via the Internet. In simple words, Internet is infrastructure while the Web is service on top of that infrastructure.

<br>

### 2. Client Vs Server
1. Client: A client is a computer hardware device or software that accesses a service made available by a server.
2. Server: A server is a computer dedicated to run services to serve the needs of other computers. Server is often located on a separate physical computer.

<br>

### 3. Peer
Peer refers to a computer on a network with the same or similar rights as another computer.

<br>

### 4. Host
Every computer connected to a network acts as a host to other peers on the network. A host must have an assigned IP address. 

<br>

### 5. Public IP Vs Private IP
1. If the server has a public IP address, it can be accessed from the web. 
2. If it has a private IP address, it can only be accessed from inside of your LAN (unless you setup port forwarding for remote access).

<br>

### 6. LocalHost
1. Localhost is always your own computer. Our computer is talking to itself when we call the localhost.
2. Localhost has the IP address 127.0.0.1. 

**What is localhost used for?**
1. Developers use the localhost to test web applications and programs. 
2. Network administrators use the loopback to test network connections.
3. Localhost can also block the hosts files. 

<br>

### 7. Bandwidth, Bitrate and Latency
1. The number of bits of data that are sent each second is known as **BitRate**. 
2. We use the term **Bandwidth** to describe the maximum bit rate of a system. Bandwidth is represented in bits, kilobits, megabits or gigabits that can be transmitted in 1 second.
3. **Latency** is the time between the sending of a data message and the receiving of that message, measured in milliseconds. For eg, My computer sends a message to the Google server. 30 milliseconds later, Google receives the message. 40 milliseconds later, my computer gets an acknowledgement from Google that it received the message. That's a total round-trip latency of 70 ms. 

<br>

**Bandwidth Vs speed**

Speed refers to the rate at which data can be transmitted, while the definition of bandwidth is the capacity for that speed. For eg, speed refers to how quickly water can be pushed through a pipe while bandwidth refers to the quantity of water that can be moved through the pipe over a set time frame.

<br>

**Why bandwidth is important?**

Greater bandwidth is essential to maintain tolerable speeds on multiple devices.

<br>


### 8. Jitter
Information is transported from your computer in data packets across the internet. They are usually sent at regular intervals. Jitter is when there is a time delay in the sending of these data packets over your network connection. This is often **caused by network congestion**.

<br>

### 9. How can we measure speed of Internet ?
Speed is a combination of bandwidth and latency.

![img](https://cdn.kastatic.org/ka-perseus-images/a993d3e7fddc5e99d6513c7e2aa44a156882e575.png)

<br>

### 10. How Computer transmit data over network ?
If a computer wants to send the number 5 to another computer, It send the number 5 over three time periods: first sending an on pulse (and waiting), then sending nothing (and waiting), then sending an on pulse. As long as the two computers agree on the time period, then they can transfer information to each other, turning binary data into signals and turning the signals back to binary data.

![img](https://cdn.kastatic.org/ka-perseus-images/697c77a549073209c372ed5938ccb542d010323b.svg)

In an electrical connection (such as Ethernet), the signal would be a voltage or current. In an optical connection (such as a fiber-optic cable), the signal would be the intensity of light. The process of turning binary data into a time-based signal is known as **line coding**.

<br>

### 11. Noice, Attenuation and Distortion
1. Noise: It refers to any external and unwanted information that interferes with a transmission signal. 
2. Attenuation: It is the loss of signal strength in networking cables or connections. This typically is measured in decibels (dB).
3. Distortion: Interruption of transmitting signals that cause an unclear reception.

<br>

### 12. ICANN (Internet Corporation for Assigned Names and Numbers)
The allocation of public IP addresses is regulated by an international organization which is the Internet Corporation for Assigned Names and Numbers (ICANN). ICANN is also responsible for the allocation of domain names called the Domain Name System (DNS).
