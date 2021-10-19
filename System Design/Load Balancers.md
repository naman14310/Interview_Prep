# Load Balancers
Load balancing refers to efficiently distributing incoming network traffic across a group of backend servers, also known as server pool.

<br>

**A load balancer performs the following functions:**
1. Distributes client requests or network load efficiently across multiple servers
2. Ensures high availability and reliability by sending requests only to servers that are online
3. Prevent overloading of requests on a single server
4. Provides flexibility to add or subtract servers on demand 
5. Reduce downtime
6. Increase Scalability


<br>

## Load Balancing Algorithms
There are two primary approaches to load balancing. **Dynamic load balancing** uses algorithms that take into account the current state of each server and distribute traffic accordingly. **Static load balancing** distributes traffic without making these adjustments. The technique chosen will depend on the type of service or application being served and the status of the network and servers at the time of the request.

<br>

### Dynamic load balancing algorithms

1. **Least connection:** Checks which servers have the fewest connections open at the time and sends traffic to those servers. This assumes all connections require roughly equal processing power.

2. **Weighted least connection:** Gives administrators the ability to assign different weights to each server, assuming that some servers can handle more connections than others.

3. **Weighted response time:** Averages the response time of each server, and combines that with the number of connections each server has open to determine where to send traffic. By sending traffic to the servers with the quickest response time, the algorithm ensures faster service for users.

4. **Resource-based:** Distributes load based on what resources each server has available at the time. Specialized software (called an "agent") running on each server measures that server's available CPU and memory, and the load balancer queries the agent before distributing traffic to that server.

<br>

### Static load balancing algorithms

1. **Round robin:** Round robin load balancing distributes traffic to a list of servers in rotation. It assumes that all application servers are the same with the same availability, computing and load handling characteristics.

2. **Weighted round robin:** Allows an administrator to assign different weights to each server. Servers who are able to handle more traffic will receive slightly more load.

3. **IP hash:** Combines incoming traffic's source and destination IP addresses and uses a mathematical function to convert it into a hash. Based on the hash, the connection is assigned to a specific server.

<br>

### Which to use When ?
<br>

1. **Round Robin** is best for clusters consisting of servers with identical specs. It won't do well in some scenarios like, if Server 1 had more CPU, RAM, and other specs compared to Server 2.

<br>

2. **Weighted Round Robin** solves the problem of round robin. The node with the higher specs should be given higher weight. Capacity isn't the only basis for choosing the Weighted Round Robin. Sometimes, we want to use it if we want one server to get a substantially lower number of connections than an equally capable server for the reason that the first server is running business-critical applications and we don't want it to be easily overloaded. 

<br>

3. There can be instances when, clients connecting to Server 2 stay connected much longer than those connecting to Server 1. This can cause the total current connections in Server 2 to pile up. As a result, Server 2's resources can run out faster. In situations like this, the **Least Connections algorithm** would be a better fit.  

<br>

4. **Weighted Least Connections** is used when we want to consider both factors: the weights/capacities of each server AND the number of clients currently connected to each server.

<br>


## Hash Based Load balancing 
Assume there is a uniformly random Request ID for each and every request. You can hash the Request ID using a hash function `h(r1)`. This hashed value can be mapped to a particular server after getting the remainder with n servers.

![img](https://miro.medium.com/max/875/1*tltkhTL8QA2ENSOR6vGn-Q.png)

Assuming hash function to be uniformly random, we can expect all servers to have a uniform load. So each of the servers will have `(Number of Requests)/n` load and the Load Factor is `1/n`.

<br>

**Problem with Normal Hashing**

Assume we need to add one more server to this system. According to example, User 1 Request ID was 44, and its hash value was 9. Since there are 5 servers now the value of the remainder is 4. Which means that User Request goes to Application Server 4. Like this, all the Request path to servers will change. As a result, all the useful cache information we had, will become useless because the mapping completely changed. This is called **Rehashing**.

<br>

### Consistent Hashing
Consistent hashing solve the problem of *rehashing* by providing a distribution scheme which does not directly depend on number of servers.

Suppose our hash function output range in between zero to INT_MAX, then this range is mapped onto the hash ring so that values are wrapped around. All requests ID's and server ID's are hashed using the same hash function and placed on the edge of the circle. To map any request with the server, we need to first locate the Request ID on the circle and move in a clockwise direction until we find a server.

<br>

![img](https://miro.medium.com/max/684/1*PdXXrVOA_k9A18BD-ZNMfQ.png)

Now we have 5 servers. According to consistent hashing rule, User 1 is on server S1, User2 is on S3, User 3 is on S2 and User 4 is on S4 server.

<br>

**Working with server Replicas**

When a server is removed or added then the only key from that server is relocated. For example, if server S3 is removed then, all pending requests of server S3 will be moved to server S4. But there is one problem, when server S3 is removed then requests from S3 are not equally distributed among remaining servers S0, S1, S2, and S4. They were only assigned to server S4 which will increase the load on server S4.

To evenly distribute the load among servers when a server is added or removed, it creates a fixed number of replicas ( known as virtual nodes) of each server and distributed it along the circle. So instead of server labels S1, S2, S3, and S4, we will have S00 S01…S09, S10 S11…S19, S20 S21…S29, S30 S31…S39 and S40 S41…S49. The factor for a number of replicas is also known as weight, depends on the situation. All keys which are mapped to replicas Sij are stored on server Si. To find a key we do the same thing, find the position of the key on the circle, and then move forward until you find a server replica. If server replica is Sij then the key is stored in server Si.

<br>

### Why do we need consistent hashing when round robin can distribute the traffic evenly?
Let's say we want to maintain user sessions on servers. So, we would want all requests from a user to go to the same server. Using round-robin won't be of help here as it blindly forwards requests in circularly fashion among the available servers. To achieve this 1:1 mapping between a user and a server, we need to use hashing based load balancers. Consistent hashing works on this idea and it also elegantly handles cases when we want to add or remove servers.


