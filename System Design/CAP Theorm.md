# CAP Theorem

[Scaler Video](https://www.youtube.com/watch?v=8UryASGBiR4) | [Primer Video](https://www.youtube.com/watch?v=R_Fxz14tr2M) | [Medium Blog](https://medium.com/codex/system-design-interview-basics-cap-vs-pacelc-cf7c5eebc313)

<br>

**The CAP theorem states that a distributed database system can at best guarantee 2 out of the 3 desired features: Consistency, Availability, and Partition tolerance.**

When working with distributed systems, there are countless databases to choose from, including MySQL, Cloudbase, Oracle, Cassandra, and so many others. CAP theorem makes it simpler to categorize the options and choose the right tool to use in different scenarios.

<br>

#### Consistency
Consistency is the guarantee that all the clients see the exact same data at the same time, irrespective of the node they connect to. This means that for the value of a write operation to be regarded as ‘successful’ once it’s performed on a single node, it must be instantly copied to all the nodes.

#### Availability
High availability in a distributed system means that the system is 100% operational at all times. Any client that makes a request gets a response, even if one or more nodes in the cluster are down.

#### Partition Tolerance
A partition refers to a break in connection between nodes within a distributed system. Partition tolerance means that the system must continue operation despite any number of partitions in the system.

<br>

### Partition Tolerance is not an option!
CAP theorem is often misinterpreted, that you can only have two out of the three characteristics at any given time. This is not true, though. The fact is that you can have all three characteristics (consistency, availability, and partition tolerance) as long as there is no network failure.

In other words, it means that partition tolerance is really not an option. Your distributed system needs to be partition tolerant in any case. Upon network failure, you can choose between consistency and availability.

<br>

1. If you choose consistency, the system will block all write operations during network failure to maintain the consistency of whole system. 
2. If you choose availability, the system will allow write operations but consistency will not be guaranteed among distributed nodes.

<br>


## Database Types Based On CAP Theorem

<br>

![img](https://miro.medium.com/max/583/1*IxtBHOXYfeTTYtg1AS8kvQ.png)

<br>

### 1. CP Database (Consistent and Partition Tolerant)
System sacrifices availability in the case of a network partition, but makes sure that consistency and partition tolerance is never compromised.

**Applications Of CP Databases:** 

In applications such as banking, eCommerce, user login, etc. availability is not as important as consistency. These applications strictly require data consistency. So it’s okay if the system is not available at all times, but if a value is returned by the node, it must be consistent.

**DataBases:** MongoDB, Apache, Hbase, Couchbase, Redis

<br>

### 2. AP Database (Available and Partition Tolerant)
All nodes continue operating in such a system, however some that are affected by the partition may return inconsistent data. Once the partition is resolved, the most recent write is replicated to each node, and consistency is reestablished.

**Applications Of AP Databases:** 

In applications such as Facebook and Instagram that work with millions of posts each day, availability is more important than consistency. It’s alright for nodes to take some time to sync and return an old value to the client until they do so, but they must be available all the time. This is called **eventual consistency**, as opposed to strict consistency, since the nodes may take some time, but will eventually replicate the latest copy of the data.

**DataBases:** Cassandra, Cosmos DB, Amazon DynamoDB, CouchDB

<br>

### 3. CA Database (Consistent and Available)
A CA database is consistent and available as long as there’s no network partition. However, Distributed systems are defined by their ability to have partitions.  Hence, a CA database can’t exist in distributed systems, apart from relational databases (SQL databases).

**Applications Of AP Databases:** 

Relational databases, such as PostgreSQL, and MySQL are the best examples of CA databases.

**DataBases:** IBM DB2, Microsoft SQL Server, Oracle, MYSQL

<br>

Note: Certain systems can be configured to operate in either CP or AP mode, depending on your application. Cassandra and Cosmos, for example, give users the option to choose between the guarantee of consistency or availability, depending on what suits the customer’s application best.

<br>

## NoSQL Databases And The CAP Theorem
The choice between consistency and availability comes when you have to pick a database type for your distributed system. NoSQL databases are a more common choice in designing distributed systems because they’re horizontally scalable (unlike SQL databases that are vertically scalable). They’re easy to scale, especially beneficial if the network is growing rapidly, with multiple interconnected nodes.

NoSQL follows two popular consistency models: ACID and BASE. ACID models guarantee consistency over availability. It’s called a ‘strong consistency model’. BASE models, which are the more popular choice of NoSQL systems, guarantee availability over consistency. BASE models offer ‘eventual consistency’.

<br>

## PACELC Theorem
The PACELC theorem states that in a system that replicates data:
1. If there is a Partition (‘P’), a distributed system can tradeoff between Availability and Consistency (i.e., ‘A’ and ‘C’);
2. Else (‘E’), when the system is running normally in the absence of partitions, the system can tradeoff between Latency (‘L’) and Consistency (‘C’).

<br>

![img](https://miro.medium.com/max/875/1*jhRZ60EgLthrOUaHYgys0Q.png)
