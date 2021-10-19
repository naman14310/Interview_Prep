# Cache

[Resource](https://www.cloudflare.com/en-in/learning/cdn/what-is-caching/)

Caching is the process of storing recently or frequently used copies of files in a nearby storage location (known as cache), so that they can be accessed more quickly. Web browsers cache HTML files, JavaScript, and images in order to load websites more quickly, while DNS servers cache DNS records for faster lookups and CDN servers cache content to reduce latency.

**Cache takes advantage of locality of reference principle: recently requested data is likely to be requested again!**

<br>

### What does a browser cache do ?
Every time a user loads a webpage, their browser has to download quite a lot of data in order to display that webpage. To shorten page load times, browsers cache most of the content that appears on the webpage, saving a copy of the webpage's content on the device’s hard drive. This way, the next time the user loads the page, most of the content is already stored locally and the page will load much more quickly.

Browsers store these files until their time to live (TTL) expires or until the hard drive cache is full. TTL is an indication of how long content should be cached. 

<br>

### What does clearing a browser cache accomplish ?
Once a browser cache is cleared, every webpage that loads will load as if it is the first time the user has visited the page. If something loaded incorrectly the first time and was cached, clearing the cache can allow it to load correctly. However, clearing one's browser cache can also temporarily slow page load times.

<br>



## What is CDN ?
A CDN, or content delivery network, caches content (such as images, videos, or webpages) in proxy servers that are located closer to end users than origin servers. Because the servers are closer to the user making the request, a CDN is able to deliver content more quickly.

<br>

**CDN cache hit and cache miss**

A cache hit is when a client device makes a request to the cache for content, and the cache has that content saved. The content will be able to load much more quickly, since the CDN can immediately deliver it to the end user 

A cache miss occurs when the cache does not have the requested content. A CDN server will pass the request along to the origin server, then cache the content once the origin server responds, so that subsequent requests will result in a cache hit.

<br>

**How long does cached data remain in a CDN server?**

When websites respond to CDN servers with the requested content, they attach the content’s TTL as well, letting the servers know how long to store it. The TTL is stored in a part of the response called the HTTP header, and it specifies for how many seconds, minutes, or hours content will be cached. When the TTL expires, the cache removes the content. Some CDNs will also purge files from the cache early if the content is not requested for a while, or if a CDN customer manually purges certain content.

<br>

**Where are CDN caching servers located?**

CDN caching servers are located in data centers all over the globe in order to be as close to end users. 

<br>



## Distributed Cache Vs Global Cache

#### Distributed Cache
In distributed cache, each of its node own a part of cached data. The cache is divided up using a consistent hashing function, such that if request node is looking for certain piece of data, it can quickly know where to look within the distributed cache to determine if the data is available.

**Advantage:** We can easily increase cache space just by adding nodes in cache pool.

**Disadvantage:** Inconsistency

<br>

#### Global Cache
All the nodes use the single cache space. This kind of caching scheme get bit complicated when number of clients and requests increases.

<br>

## Cache Invalidation
If data is modified in database, it should be invalidated in the cache otherwise it becomes inconsistent. Following 3 schemes are majorly used:

#### 1. Write Through Cache
Data is written into the cache and the corresponding database at a same time. It minimizes risk of data loss, since every write operation must be done twice before returning success to client, this scheme has disadvantage of higher latency for write operations.

#### 2. Write around Cache
Data is written directly to permanent storage, bypassing the cache. This can reduce the cache being flooded with write operations that will not subsequently be read but has disadvantage that read request for recently written data will create a cache miss. 

#### 3. Write Back Cache
Data is written to cache alone and completion is immediately confirmed to client. The write to permanent storage is done after specified intervals. This results in low latency and high throughput for write intensive applications. However this scheme comes with the risk of data loss in crash. 








