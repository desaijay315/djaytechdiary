## Thinking in Redis: A Quick Introduction

Redis is one of the most known and widely used NoSQL in-memory data structure used by many developers and companies across the world. 

In this article, we will learn all about Redis, it's fundamentals and core concepts, and
many more...

### What is Redis?

Redis is defined as an open-source in-memory data structure store which can be used as a database, cache and message broker. It is considered as a NoSQL key-value store. 

It's in-memory feature make Redis extremely fast access to data and therefore used frequently as a Cache or as a secondary database supporting the primary one. 

You can also think of Redis as a  [MongoDB](https://djaytechdiary.com/mongodb-a-beginners-guide) database, but MongoDB is a disk-based document store while Redis is a memory-based datastore

### So, What is different?

- Redis is different than SQL datastore. It doesn't have any schemas and columns
- Data retrieval with SQL requires a scan on the entire table or scan on the entire index. Redis provides direct retrieval commands and provides the ability to get the data which you have stored in the key with its respective data
- It takes advantage of speed
- It does not have any query engine
- Developers can decide how to store and retrieve data

### What Redis is NOT?

- Redis is not a replacement for Relational Databases nor Document store
- It can be used as a secondary database complementing the primary one. 


**Let's discuss some of the many features of Redis which can help us in Scaling Redis:**

- Persistence
- Replication
- Performance
- Failover
- Security

### Persistence

- Redis provides two mechanisms to deal with persistence: Redis database snapshots(RDB) and Append-only files(AOF).
- **Memcache** used volatile cache, therefore the data does not get persisted and it lost with a restart. Redis uses built-in persistence and will not disappear with a restart. 

### Replication

- Redis supports replication of in-memory content for high-availability production applications. 
- Master instance ensures, that one or more replicas(slaves instance) copy the exact same data.
- Replication becomes important to ensure that data is available in the event of a failure of a database component.



### Performance

- Redis Enterprise can perform more than 500,000(approx) operations per second with sub-millisecond latency and can also achieve 50 million operations per second with the same performance on only 26 compute nodes.
- The performance of Redis coupled with search features like autocomplete and result in highlighting makes the entire user experience better.


### Failover

- This can be addressed manually
- Automatic with Redis Sentinel(for master-slave topology)
- Automatic with Redis Cluster(for the cluster topology)

### Security

- Redis was optimized for max performance rather than max security. We can set up a tiny layer of authentication. All the queries sent to the Redis would then send AUTH command with the request for authentication
- We can store the password in **redis.conf** and we need to make sure it's a strong and secure password 
- Access to the port should be denied and not be exposed externally
- We might require to bind the IP and remote port to open remote connection, but Firewall configuration is must which prevents access remotely and control who connects to the port.
- Redis is designed to be accessed by trusted clients inside trusted environments and like we said, that it should be not be exposed externally, Encryption of data at rest isn't supported by Redis yet.


### Data Structures

- Redis supports several data structures. In fact, it may be helpful to think of Redis as a data structures store rather than a simple key/value NoSQL store.
- Redis keys can contain data structures such as strings, hashes, lists, sets and sorted sets, bit arrays, streams, hyperloglogs.
- Every data structure for Redis can hold 2³² elements i.e. hash, list, set, and sorted sets can hit the peak with 2³² elements.
- Beside this data structures, Redis also supports the Publish/Subscribe( [PubSub](https://djaytechdiary.com/graphql-subscriptions-with-redis-pubsub)) pattern which can be used for more data-intensive applications.
- All operations are atomic(no two commands and be used at the same time).
- It executes most commands in O(1) and with minimal lines of code.

### Some of the use cases for Redis:

- **Caching** - Cache most frequently used pieces of data. It loads the data from slower data sources into Redis and keeps the data into RAM for faster retrieval.
- **Provide rate-limiting** - It can be used to rate-limit API endpoint by real-time tracking user and their activity. 
-  [Pub/Sub](https://djaytechdiary.com/graphql-subscriptions-with-redis-pubsub)
- User sessions - Use it for session storage
- Realtime analysis - Data that needs to be processed can be stored effectively in a compact manner
- Social Apps
- Search
- Full-Text Search
- Geospatial and time-series data
- Short live items, with ttl

### Redis Clients

> 
Redis has a client for about every popular programming language. You can check  more on Redis clients [here](http://redis.io/clients) 


### Conclusion

In this article, we learned what is Redis, some of its features that will help you get started.

In upcoming articles, we would learn more on basic and advanced Redis commands, use different data structures, Integrate Redis with Node.js APIs and learn more on best practices.

If you liked it please leave some love to show your support. Also, leave your responses below and reach out to me if you face any issues.