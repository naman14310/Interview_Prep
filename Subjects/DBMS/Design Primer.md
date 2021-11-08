# DataBase

<br>

## Relational database management system (RDBMS)
A relational database like SQL is a collection of data items organized in tables.

**ACID** is a set of properties of relational database transactions.

1. Atomicity - Each transaction is all or nothing
2. Consistency - Any transaction will bring the database from one valid state to another
3. Isolation - Executing transactions concurrently has the same results as if the transactions were executed serially
4. Durability - Once a transaction has been committed, it will remain so

<br>

## How to Scale Relational Databases ?
There are many techniques to scale a relational database: master-slave replication, master-master replication, federation, sharding, denormalization, and SQL tuning.

<br>

### Replication
Replication not only improves the overall reliability of data, it also helps performance by spreading the workload across multiple replicas. While read-only request can be dispatched to any replicas, update request is more challenging because we need to carefully co-ordinate the update which happens in these replicas.

<br>

#### Master-Slave Replication
The master serves reads and writes, replicating writes to one or more slaves, which serve only reads. Slaves can also replicate to additional slaves in a tree-like fashion. If the master goes offline, the system can continue to operate in read-only mode until a slave is promoted to a master or a new master is provisioned.

<br>

![img](https://github.com/donnemartin/system-design-primer/blob/master/images/C9ioGtn.png)

<br>

#### Master-Master Replication
Both masters serve reads and writes and coordinate with each other on writes. If either master goes down, the system can continue to operate with both reads and writes.

<br>

![img](https://github.com/donnemartin/system-design-primer/blob/master/images/krAHLGg.png)

<br>

#### Disadvantages of Replication

1. There is a potential for loss of data if the master fails before any newly written data can be replicated to other nodes.
2. Most master-master systems are either loosely consistent (violating ACID) or have increased write latency due to synchronization.
3. Conflict resolution comes more into play as more write nodes are added and as latency increases.
4. Replication adds more hardware and additional complexity.

<br>

### Federation
Federation (or **functional partitioning**) splits up databases by function. For example, instead of a single, monolithic database, you could have three databases: forums, users, and products.

<br>

![img](https://github.com/donnemartin/system-design-primer/blob/master/images/U3qV33e.png)

<br>

#### Advantages
1. Less read and write traffic to each database and therefore less replication lag. 
2. More cache hits due to improved cache locality. 
3. With no single central master serializing writes you can write in parallel, increasing throughput.

<br>

#### Disadvantages
1. Federation is not effective if your schema requires huge functions or tables.
2. Overhead in determining which database to read and write.
3. Joining data from two databases is more complex with a server link.

<br>

### Sharding
Sharding distributes data across different databases such that each database can only manage a subset of the data. Taking a users database as an example, as the number of users increases, more shards are added to the cluster.

<br>

#### Advantages
1. Sharding results in less read and write traffic, less replication, and more cache hits. 
2. Index size is also reduced, which generally improves performance with faster queries. 
3. If one shard goes down, the other shards are still operational, although you'll want to add some form of replication to avoid data loss. 
4. Like federation, there is no single central master serializing writes, allowing you to write in parallel with increased throughput.

<br>

#### Disadvantages
1. Results in complex SQL queries.
2. Data distribution can become lopsided in a shard. For example, a set of power users on a shard could result in increased load to that shard compared to others.
Rebalancing adds additional complexity. A sharding function based on consistent hashing can reduce the amount of transferred data.
3. Joining data from multiple shards is more complex.

<br>

### DeNormalization
Denormalization attempts to improve read performance at the expense of some write performance. Redundant copies of the data are written in multiple tables to avoid expensive joins. In most systems, reads can heavily outnumber writes 100:1 or even 1000:1. A read resulting in a complex database join can be very expensive, spending a significant amount of time on disk operations.

<br>

#### Disadvantages
1. Data is duplicated.
2. A denormalized database under heavy write load might perform worse than its normalized counterpart.

<br>

## NO-SQL DataVases
NoSQL is a collection of data items represented in a key-value store, document store, wide column store, or a graph database. Data is denormalized, and joins are generally done in the application code. Most NoSQL stores lack true ACID transactions and favor eventual consistency.

**BASE** is often used to describe the properties of NoSQL databases. In comparison with the CAP Theorem, BASE chooses availability over consistency.
1. Basically available - the system guarantees availability.
2. Soft state - the state of the system may change over time, even without input.
3. Eventual consistency - the system will become consistent over a period of time, given that the system doesn't receive input during that period.

<br>

### Key-value store
`Abstraction: hash table`

A key-value store generally allows for O(1) reads and writes and is often backed by memory or SSD. Data stores can maintain keys in lexicographic order, allowing efficient retrieval of key ranges. Key-value stores can allow for storing of metadata with a value.

Key-value stores provide high performance and are often used for simple data models or for rapidly-changing data. A key-value store is the basis for more complex systems such as a document store, and in some cases, a graph database.

<br>

### [Redis Architecture](http://qnimate.com/overview-of-redis-architecture/)

<br>

### Document store
`Abstraction: key-value store with documents stored as values`

A document store is centered around documents (XML, JSON, binary, etc), where a document stores all information for a given object. Document stores provide APIs or a query language to query based on the internal structure of the document itself. 

Some document stores like MongoDB and CouchDB also provide a SQL-like language to perform complex queries. DynamoDB supports both key-values and documents. Document stores provide high flexibility and are often used for working with occasionally changing data.

<br>

### [Key Store Vs Document Store](https://stackoverflow.com/questions/3046001/what-does-document-oriented-vs-key-value-mean-when-talking-about-mongodb-vs-c)

<br>

### Graph database
`Abstraction: graph`

In a graph database, each node is a record and each arc is a relationship between two nodes. Graphs databases offer high performance for data models with complex relationships, such as a social network. They are relatively new and are not yet widely-used. Many graphs can only be accessed with REST APIs.

<br>

![img](https://github.com/donnemartin/system-design-primer/blob/master/images/fNcl65g.png)

<br>


## [SQL Vs NoSQL](https://www.sitepoint.com/sql-vs-nosql-differences/)

### Reasons for SQL
1. Structured data
2. Strict schema
3. Need for complex joins
4. Transactions
5. More established: developers, community, code, tools, etc
6. Lookups by index are very fast

<br>

### Reasons for NoSQL

1. Semi-structured data
2. Dynamic or flexible schema
3. No need for complex joins
4. Very data intensive workload

<br>

### Sample data well-suited for NoSQL:
1. Rapid ingest of clickstream and log data
2. Leaderboard or scoring data
3. Temporary data, such as a shopping cart
4. Metadata/lookup tables




