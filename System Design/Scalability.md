# Scalability

<br>

## What is Scalability ?
Scalability is the ability to expand from the existing configuration of the system for handling increasing amount of load. It can be done by either adding extra hardware or by upgrading the current system configuration. There are two different ways to accomplish scaling:
1. Horizontal Scaling
2. Vertical Scaling

<br>

![img](https://demo.cloudswitches.com/wp-content/uploads/2019/12/redswitches-blog-scaling-image.jpg)

<br>

### Horizontal Scaling
1. Horizontal scaling is a strategy used to enhance the performance of server node by adding more instances of server to your existing pool so that the load can be equally distributed.
2. Horizontal scalability is achieved with the help of a distributed file system, clustering, and load balancing.
3. It follows the partitioning of the data in which each node contains only one part of the data.
4. Horizontal scaling is also known as scaling out.

<br>

**Example of horizontal scaling: Cassandra and MongoDB.**

<br>

**Advantages:**
1. Easier to run fault-tolerance
2. Easy to upgrade
3. Infinite Scaling capacity
4. Improved resilience due to the presence of discrete, multiple systems

<br>

**Disadvantages:**
1. Architectural design is highly complicated
2. High utility costs (cooling and electricity)
3. Extra networking equipment such as routers and switches are required

<br>

### Vertical Scaling
1. Vertical Scaling is an attempt to increase the capacity of a single machine. Here the resources such as processing power, storage, memory etc. are added to an existing work unit.
2. It can enhance your server without manipulating your code. 
3. In vertical-scaling, there is no partitioning of the data. It all resides on a single node.
4. Vertical Scaling strategy is widely used in applications of small and mid-sized companies.
5. Vertical Scaling is also called the Scale-up approach.

<br>

**Example of Vertical Scaling: MySQL and Amazon RDS.**

<br>

**Advantages:**
1. Easy Implementation.
2. Low utility costs (cooling and electricity)

<br>

**Disadvantages:**
1. Limited Scaling.
2. The risk for downtime is much higher than horizontal scaling.
3. Greater risk of hardware failures.

<br>

### [Which is right for our App ?](https://www.missioncloud.com/blog/horizontal-vs-vertical-scaling-which-is-right-for-your-app)

<br>

## Performance Vs Scalability
Performance refers to the capability of a system to provide a certain response time, serve a defined number of users or process a certain amount of data. So performance is a software quality metric. Whereas, Scalability referes to the characteristic of a system to increase performance by adding additional ressources.

The system is scalable, if adding additional resources really helps to improve performance. Whether your system is scalable or not depends on your architecture. If we want our systems to be scalable we have to take this into consideration right from the beginning of development and also monitor throughout the lifecycle. 


