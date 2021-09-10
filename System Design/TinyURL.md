
# URL Shortner service (Tiny URL)
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

System will be read heavy. There will be more redirection requests (approx 10 times) then url-shortening requests. Assuming 200 QPS for shortning reqs and 1K QPS for redirection reqs, we need 15 TB of storage if we store each url for a span of 5 years with each object of size 500 bytes.

<br>

### System APIs

1. registerURL (original_url, custom_alias(optional))
2. deleteURL (key)

<br>

### Database Used

-NoSQL

Reason: Since we are likely going to store billions of rows and we don't need to use relationships between objects, So a NoSQL database like MongoDB or Dynamo DB will be better choice which would also be easier to scale. 

<br>

### Design

**Encoding scheme**

We can use Base64 encoding (A-Z, a-z, 0-9, -, .)

```
Scale estimation
----------------

Using 6 letters, we can generate = 64^6 urls ~= 68.7 billion
Using 8 letters, we can generate = 64^8 urls ~= 281 trillion
```

<br>

**What will be the factors affecting the length of our URL?**
1. Traffic expected - how many URLs we might have to shorten per second
2. For how long do we need to support these URLs

<br>

**Architecture**



