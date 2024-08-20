# Load Balancer
#### Techniques
* Round robin: send requests to each server one by one
* Least connections / response time: think of response times as waiting times due to long queues
* Hashing: serverIndex = hash(key) % N, where key is client IP or username. Useful only with fixed no. of servers, practically impossible. Need to remap whole set of keys.

##### Consistent hashing
* Remap only k/n keys on average, where k is the number of keys, and n is the number of slots.
• Map servers and keys on to the ring using a uniformly distributed hash function (SHA-1’s hash space goes from 0 to 2^160 - 1). No modulo operation.
• To find out which server a key is mapped to, go clockwise from the key position until the first server on the ring is found.
* During server addition/removal, travel anticlockwise from server location until previous server is found, and remap all keys in between.
* Issue: non-uniform key distribution, different sized partitions for each server
* To solve above issue, use multiple virtual nodes/dummy nodes,representing each server, distributed across the ring 
* The standard deviation is between 5% (200 virtual nodes) and 10% (100 virtual nodes) of the mean.
* Where is it used: Partitioning component of Amazon’s Dynamo database, Data partitioning across the cluster in Apache Cassandra [4]
