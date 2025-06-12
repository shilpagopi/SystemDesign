# Caching
* Storing results of an expensive computation or network calls or database queries or asset fetching,so you don't have to redo it.
* Generally, caching is used for read-heavy systems (like social media). The 80/20 rule: You want to store 80% of read requests in 20% of storage (or memory). Store popular data.
* A CDN (content delivery network) is something that's commonly used to deliver cached data.
* Types of cache: storage cache (eg. CDN) or metadata cache.

### Features
* Amount of data : TBs
* Eviction Strategy
* Cache happens in main memory
* Type of caching patterns:
  * Cache aside pattern (most popular):
     * app fetches data from the cache, and if the data is not found (also known as a “cache miss”) it will fetch data from the database, then it will put that data back to the cache before returning the query back to the user.
     * we only cache the data that we need
     * if many cache misses, then more work than regular flow   
  * Write through cache : This is a caching system where writes go through the cache and write is confirmed as success only if writes to DB and the cache BOTH succeed. This is really useful for applications which write and re-read the information quickly. However, write latency will be higher in this case as there are writes to 2 separate systems. (App writes directly to cache.)
  * Write around cache : This is a caching system where write directly goes to the DB. The cache system reads the information from DB incase of a miss. While this ensures lower write load to the cache and faster writes, this can lead to higher read latency incase of applications which write and re-read the information quickly.
  * Write back cache : This is a caching system where the write is directly done to the caching layer and the write is confirmed as soon as the write to the cache completes. The cache then asynchronously syncs this write to the DB. This would lead to a really quick write latency and high write throughput. But, as is the case with any non-persistent / in-memory write, we stand the risk of losing the data (and thereby suffering from inconsistency) incase the caching layer dies. We can improve our odds by introducing having more than one replica acknowledging the write ( so that we don’t lose data if just one of the replica dies ).

### Use client side cache

### Cache invalidation: 
* “Least Recently Used” (or LRU) favours most popular data, and thereby less cache misses.
* Stale data handling: “Time To Live” (or any other expiry pattern)

### Estimation
* A production level caching machine would be 72G or 144G of RAM (for applications using main memory)
* Min. number of machine required = 30 TB / 72G which is close to 420 machines.

### Design Goals
* Latency - Is this problem very latency sensitive (Or in other words, Are requests with high latency and a failing request, equally bad?). For example, search typeahead suggestions are useless if they take more than a second.
* Consistency - Eventually consistent?
* Availability

### Redis
* Low latency cache (works as a key-value store)
* Redis 7.4 and later: Supports Hash Field Expiration (HFE) (for automatic expiry of hashes or specific fields after TTL) - used for rate limiting or time series based counting.

### Memcached

Memcached is an in-memory, distributed key-value store designed for speed and simplicity, primarily used as a caching layer.

Key Characteristics:

In-Memory: Stores data entirely in RAM, making it extremely fast.
Key-Value Store: Simple interface; you store and retrieve data based on a unique key.
Distributed: Data can be spread across multiple Memcached servers. Clients use a consistent hashing algorithm (or similar) to determine which server a key belongs to.

Simple API: Basic operations like set, get, delete, add, incr, decr.
Least Recently Used (LRU) Eviction: When memory is full, Memcached automatically evicts the least recently used items to make space for new ones.
No Persistence: Data is lost if the Memcached server restarts or crashes. This is a fundamental trade-off for speed and simplicity.
Multi-threaded (since recent versions): While Redis is single-threaded, Memcached uses multiple threads to handle network I/O, which can be advantageous in some high-concurrency scenarios for throughput, though it doesn't offer the atomic guarantees of Redis for complex operations.
How is Memcached Used in System Design Scenarios?
The core use case for Memcached is to reduce the load on slower backend systems (primarily databases) by caching frequently accessed data.

Here are some common scenarios to discuss in an interview:

Database Query Caching (Most Common):

Scenario: A web application (e.g., e-commerce, social media, news portal) frequently performs the same complex database queries to fetch data for common requests (e.g., "get product details for top-selling items," "fetch a user's profile," "retrieve the most recent news articles").
Problem Solved: Directly querying the database for every request can become a bottleneck as user traffic scales. Database queries are expensive (disk I/O, CPU for joins/filters).
Memcached Usage:
When an application needs data, it first checks Memcached.
Cache Hit: If the data is found in Memcached (a "cache hit"), it's returned immediately to the user, bypassing the database. This is extremely fast (microseconds).
Cache Miss: If the data is not in Memcached (a "cache miss"), the application queries the database.
Once the data is retrieved from the database, it's stored in Memcached with an appropriate TTL (Time-To-Live) or expiration policy, before being returned to the user.
Invalidation: When the underlying data in the database changes (e.g., a product's price is updated), the corresponding entry in Memcached is explicitly deleted or overwritten (set) to ensure data consistency.
Benefit: Dramatically reduces database load, improves response times, and allows the application to handle more concurrent users.
Session Caching for Web Applications:

Scenario: A scalable web application running on multiple servers needs to store user session data (e.g., login status, shopping cart contents, user preferences).
Problem Solved: If session data is stored only on individual web servers, users might lose their sessions if they hit a different server (e.g., due to load balancer routing). Storing sessions in a database is too slow.
Memcached Usage:
When a user logs in or performs an action, their session data is stored in Memcached with their session ID as the key.
Any web server instance can then retrieve the session data from Memcached, ensuring session stickiness across multiple servers.
The data is stored with a relatively short TTL (e.g., 30 minutes to a few hours) to manage memory and ensure inactive sessions expire.
Benefit: Enables horizontal scaling of web servers without session issues, improving availability and fault tolerance.
API Response Caching:

Scenario: An API gateway or backend service serves common, non-personalized API responses that don't change frequently (e.g., a list of categories, static configuration data, aggregated statistics that refresh every few minutes).
Problem Solved: Repeatedly generating these responses can consume server resources.
Memcached Usage: The API gateway or service stores the full API response (often as a JSON string) in Memcached. Subsequent identical requests hit the cache.
Benefit: Speeds up API responses and reduces load on downstream services.
Lookup Tables and Reference Data:

Scenario: Applications often need to quickly look up static or slowly changing reference data (e.g., country codes, currency conversion rates, error message definitions).
Problem Solved: Loading this data from a database or file system on every request is inefficient.
Memcached Usage: This data is loaded into Memcached once (or refreshed periodically) and then served from memory.
Benefit: Extremely fast lookups for frequently used static data.
Distributed Counters/Flags (with care):

Scenario: Tracking simple counts (e.g., "number of views for an article") or boolean flags across multiple application instances.
Problem Solved: Using a database for every increment can be slow and lead to contention.
Memcached Usage: Memcached provides incr and decr commands. However, because Memcached doesn't guarantee persistence and isn't designed for strong consistency, this use case is typically for "eventual consistency" or "approximate counts" where slight loss of data on restart is acceptable. For critical, atomic counters, Redis or a database would be preferred.
Benefit: Fast, lightweight distributed counters for non-critical metrics.
Memcached Deployment in System Design:

Dedicated Servers/Instances: Memcached runs on dedicated servers or cloud instances (e.g., AWS ElastiCache for Memcached, Google Cloud Memorystore for Memcached).
Client Libraries: Application clients use libraries that implement consistent hashing or a similar sharding strategy to distribute keys across the Memcached cluster. This client-side routing means there's no central coordinator, simplifying the Memcached server architecture.
Scaling: To scale Memcached, you add more nodes to the cluster. The client libraries handle re-hashing or re-distribution of keys.
Memcached vs. Redis (Common Interview Comparison):

While Memcached excels at simple key-value caching, it's often compared to Redis. Key differences to highlight:

Data Structures: Memcached is purely key-value (strings). Redis offers rich data structures (Lists, Sets, Hashes, Sorted Sets, etc.).
Persistence: Memcached is non-persistent. Redis offers persistence options (RDB, AOF).

Atomicity: Memcached offers atomic operations on single commands. Redis offers MULTI/EXEC transactions and Lua scripting for multi-command atomicity.
Replication/High Availability: Memcached does not have built-in replication; HA is typically handled at the application level (e.g., if a node fails, clients try other nodes or refresh from DB). Redis offers master-replica replication and clustering.

Complexity: Memcached is simpler. Redis is more feature-rich and can serve as a primary data store for certain use cases.

When to choose Memcached:

You primarily need a simple, high-performance, in-memory cache.
You don't need complex data structures.
You are okay with data loss on restart/failure (as it's a cache, the source of truth is elsewhere).
You need extreme throughput for get operations.
By explaining these scenarios and characteristics, you demonstrate a solid understanding of Memcached's role and trade-offs in system design.
