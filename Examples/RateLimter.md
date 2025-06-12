# Rate Limiter

### FR
* Limits the number of events an entity (user, device, IP, etc.) can perform in a particular time window.: Is it for user or API Level? User level possible only after authentication.(Rate limiting by IP would mean a user has huge number of IPv6 addresses from a single computer, and our system will be overloaded. If we have to rate limit on the login API itself, we need to go by IP-based rate limiting. The weakness of this rate-limiting would be that a hacker can perform a denial of service attack against a user by entering wrong credentials up to the limit; after that the actual user will not be able to log-in.) 
* Takes into account all servers

### NFR
* Availability >> Consistency
* Consistent when checking limits
* Low latency rate limiting verification (not affecting normal user experience)

### Core data entities
* user/api, counter (shard by api or user, based on quesiton requirement)
* two tables if we support reduced limits after crossing one major threshold criteria

### BOE:
* Estimates: Let’s assume ‘UserID’ takes 8 bytes. Let’s also assume a 2 byte ‘Count’, which can count up to 65k, is
sufficient for our use case. Although epoch time will need 4 bytes, we can choose to store only the
minute and second part, which can fit into 2 bytes. Hence, we need a total of 12 bytes to store a user’s
data:

8 + 2 + 2 = 12 bytes

Let’s assume our hash-table has an overhead of 20 bytes for each record. If we need to track one
million users at any time, the total memory we would need would be 32MB:
(12 + 20) bytes * 1 million => 32MB
Easily fit on single server memory wise. But if he handle 10 requests/sec from each of 1M users: 10M QPS will be our bottleneck.So, use distributed Redis cache.

### HLD
* Use Redis Cache for low latency lookup: store userid, counts. Redis's inherent single-threaded command execution ensures atomicty
* Fixed window vs Sliding window : Fixed window means user can send k requests at 11:59 and another k at 12:01. unnecessary spikes.
  For sliding window, user Redis SortedSet to popout stale entries outside current window, then check limits, and insert if check passes, send accept/reject.
  
### Deepdives
* Sliding window using sortedset
* How to allow a reduced limit after main limit exceeds, say 10 req/min after 500 req per hour?
  Option 1: Maintain separate "fixed" 1-min windows (compromises on consistency within less than a min) and sum of past 60 hr windows or 
  Option 2: Maintain last 1min table (only a small fraction of duplication of entries, which are anyway not much memory overload)
  
###### Throttling
It is the process of controlling the usage of the APIs by customers during a given
period. Throttling can be defined at the application level and/or API level. When a throttle limit is
crossed, the server returns HTTP status “429 - Too many requests".
* Hard Throttling: The number of API requests cannot exceed the throttle limit.
* Soft Throttling: In this type, we can set the API request limit to exceed a certain percentage. For
example, if we have rate-limit of 100 messages a minute and 10% exceed-limit, our rate limiter will
allow up to 110 messages per minute.
* Elastic or Dynamic Throttling: Under Elastic throttling, the number of requests can go beyond the
threshold if the system has some resources available. For example, if a user is allowed only 100
messages a minute, we can let the user send more than 100 messages a minute when there are free
resources available in the system.

