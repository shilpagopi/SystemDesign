# Caching
* Storing results of an expensive computation or network calls or database queries or asset fetching,so you don't have to redo it.
* Generally, caching is used for read-heavy systems (like social media). The 80/20 rule: You want to store 80% of read requests in 20% of storage (or memory). Store popular data.
* A CDN (content delivery network) is something that's commonly used to deliver cached data.

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

  
