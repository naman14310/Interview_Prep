
# URL Shortner Service (Tiny URL)
It will provide short aliases redirecting to long URLs. A shortened version of url will save lot of space whenever we use it.


**Similar services**
1. tinyurl.com
2. bit.ly
3. goo.gl
4. 2020.fm

<br>

### Requirements

**Functional Requirement**
1. Generate shorter and unique aliases
2. Redirect to original link on click
3. Give option to pick custom alias (if available)
4. Links will expire after specific timespan


**Non Functional Requirement**
1. System should be highly available. Because is service gets down, all URL's redirections will get failed.
2. Lower latency
3. Shortened links should not be predictable.

<br> 

### Capacity Estimation

System will be read heavy. There will be more redirection requests (approx 10 times) then url-shortening requests. 

Assuming 200 QPS for shortning reqs and 1K QPS for redirection reqs, we need 15 TB of storage if we store each url for a span of 5 years with each object of size 500 bytes.

<br>

### System APIs

1. registerURL (original_url, custom_alias(optional))
2. getURL (short_url)
3. deleteURL (original_url)

<br>

### Database Used

-NoSQL

Reason: Since we are likely going to store billions of rows and we don't need to use relationships between objects, So a NoSQL database like MongoDB or Dynamo DB will be better choice which would also be easier to scale. 

<br>

### Encoding scheme

We can use Base62 encoding (A-Z, a-z, 0-9)

```
Scale estimation
----------------

Using 7 letters, we can generate = 62^7 urls ~= 3.5 trillion
For 1K reqs per second, It will take 110 years to exhaust all urls
```

<br>

**What will be the factors affecting the length of our URL?**
1. Traffic expected - how many URLs we might have to shorten per second
2. For how long do we need to support these URLs

<br>

**Code**

```py
def to_base_62(deci):
    s = '012345689abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
    hash_str = ''
    while deci > 0:
       hash_str= s[deci % 62] + hash_str
       deci /= 62
    return hash_str
```

<br>


### Techniques to Generate and Store TinyURL 

Following are the 3 approaches to generate unique short url's:
1. Generate random number of base62 and check DB if it exists
2. Pick first 43 bits of MD5
3. Use Counter (Single machine, multiple machines, range based)


<br>

#### 1. Generating Random numbers and check DB everytime
We'll simply assign random numbers to every req and checks database everytime till we get the number which was not used before. A good random genrator will lower down the lookup to one or two search operations. 

We will be running multiple instances of this short URL service, say SUS1, SUS2, SUS3, etc. Now there is a possibility that more than one of them will end up generating the same number, which means one short URL will now point to two long URLs, which shouldn't happen. This is known as a collision. Use other approaches to minimize or remove collision.

<br>

#### 2. Use MD5 or any other hashing functions
It will generate a hash value for every incoming long url and then use few bits of that value to generate short url. More number of bits, lesser will be the collisions. But It'will not completely remove the collisions. 

Advantage over random numbers : 

It will generate same short url for two same long urls (as their hash value will be equal) which helps us in saving space. 

<br>

#### 3. Using Counters (Redis)
We will use counters, which will completely remove the collisions by assigning tokens (incremented in some sequence) to each req. Counter will not generate same token again once it get used. 

<br>

**Single Host**

We will use single counter to assign tokens. It will start counting from 1 and keep incrementing the count for each request before responding back. With this unique number, we can generate a unique URL by converting it to base 62.

Challenges:
1. Every machine will be interacting with Redis, which will increase the load on Redis.
2. Also, the Redis here becomes a single point of failure, which is a big NO! 

<br>

**Multiple Host**

We'll use multiple instances of Redis. This will give us better performance and higher availability!

![img](https://www.codekarle.com/images/blog-images/tiny_url_%20intermediate_solution.jpg)

Challenges:
1. Due to involvement of multiple instances, their are chances for collision. It can avoided by assigning series but it fails when we want to assing new servers.

<br>

**Range Based (using Zookeeper)**

Zookeeper is a token service will run on a single-threaded model and cater to only one machine at a time so that each machine has a different range. All services will only interact with this zookeeper on startup and when they are about to run out of their range, hence it will be dealing with a very minimal load. It can managed using zookeeper. 

Say we have 3 short URL services, SUS1, SUS2, SUS3. On startup say SUS1 has range 1-1000, SUS2 has range 1001-2000 and SUS3 has range 2001-3000. When SUS1 runs out of its range token service will make sure it gets a range that hasn’t been assigned to another machine. One way to ensure this would be to maintain these ranges as records and keeping an “assigned” flag against them. When a range is assigned to a service, the “assigned” flag can be set to true.

![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20210214204810/Zookeeper-URL-Shortening-Service.png)

Advantages over other approaches:
1. No collision because all services are working within their range.
2. We can easily assign new servers without affecting others. 
3. Zookeeper service is distributed across geographies to reduce latency and also to make sure it is not a single point of failure.
4. If in case, one of the servers goes down then we will only lose a million combinations in Zookeeper but since we have 3.5 trillion combinations we should not worry about losing this combination.

<br>

**Why Zookeeper ?**

It solve the various challenges of a distributed system like a race condition, deadlock, or particle failure of data. Zookeeper is basically a distributed coordination service that manages a large set of hosts. It keeps track of all the things such as the naming of the servers, active servers, dead servers, configuration information of all the hosts. It provides coordination and maintains the synchronization between the multiple servers. 

<br>

**Problem with all counter approaches**

They will genrate urls and sequences with might be a threat to some security purposes. 

Solution: We can append some extra random bits with url generated with counters.

<br>

### Use of Cache
We can use cache for providing fast redirections to some frequently used URL's.

<br>

### How do we scale ?
1. We can either spin up multiple instances of the MySQL instances and distribute them across the map.
2. We could simply increase the length of our range. That would mean that machines will approach the token service at a much lower frequency.


<br>

### References
1. [Tushar Roy](https://www.youtube.com/watch?v=fMZMm_0ZhK4)
2. [CodeKarle](https://www.codekarle.com/system-design/TinyUrl-system-design.html)
3. [GFG](https://www.geeksforgeeks.org/system-design-url-shortening-service/)
