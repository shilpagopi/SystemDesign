# Indexing and Sharding

## Indexing
* Indexing is a mechanism by which the underlying data is mapped for faster retrieval.
* Any data accessing mechanism: Fetch the block of data from the hard disk (secondary/permanent storage) to the primary memory (e.g. RAM). Look for the desired data in the RAM.
* Dense index: a key-value mapping for each of the db records vs.sparse index (only for a subset of the records)
* Multilevel indexing: usually implemented using self-adjusting B trees or B+ trees

## What is a Composite Index?
* A composite index is a single index data structure that stores the values of multiple columns in a specific order. When you create a composite index, the database sorts the data based on the values of the first column, then by the second column within the first column's sorted values, and so on.
Example Syntax (conceptual):CREATE INDEX idx_lastname_firstname ON Employees (LastName, FirstName);
* Beneficial for queries that filter or sort data based on multiple columns sya, in WHERE or ORDER BY or in SELECT col1,col2.  
* Order of columns is crucial. Place the column with the highest cardinality (most unique values) first in the index, Columns used in equality conditions (=)
* Not recommended if merging over ranges of both columns are expected

## Sharding vs. Partitioning
* Sharding:  across different physical servers. Partition: ordering within each server.
  <img width="722" alt="image" src="https://github.com/user-attachments/assets/cda184f1-81c9-4d83-a2a6-7869ca13dba7" />
  Eg. In Kafka, the partition key determines which partition a message will be written to, ensuring message ordering within that partition

## Sharding
shard key:one or more columns in your table that determine how data is distributed

Common Sharding Strategies and Lookup Mechanisms:
1. Hash-Based Sharding
Pros: Even distribution of data, which helps prevent hot spots.
Cons: Range queries (e.g., "all users created between X and Y") are inefficient leading to a "scatter-gather" query (querying all shards and merging results).
2. Range-Based Sharding
How it works: Data is distributed based on user-defined ranges of the shard key's value
Pros: Very efficient for range queries
Cons: Can lead to "hot spots" 
3. Directory-Based Sharding (Lookup Table / Shard Map)
How it works: A separate "lookup table" or "directory service" maintains a mapping between the shard key and the actual shard where the data resides. This lookup table itself needs to be highly available and potentially sharded or replicated.
Pros: Most flexible. Allows for easy rebalancing (moving data between shards and updating the map) and adding/removing shards without changing the sharding logic in the application. Can use arbitrary keys for sharding.
Cons: Adds an extra lookup step (an additional network hop and database query) for every read/write, increasing latency. The lookup service itself can become a bottleneck or a single point of failure if not designed robustly.

##### The Routing Layer
* Application-level sharding: The application code itself contains the logic to calculate the shard (e.g., applying the hash function) and connect to the correct database instance.
* Database-aware drivers/proxies: Some database drivers or middleware (like Vitess for MySQL) understand the sharding scheme and route queries automatically. The application interacts with a single logical endpoint.
* Proxy layer: A separate service sits in front of the database shards, acting as a router. The application sends all queries to this proxy, which then forwards them to the appropriate shard. This decouples the sharding logic from the application.

##### Handling Queries Without a Shard Key
* Global Secondary Indexes: To make non-shard-key lookups efficient, you often need global secondary indexes. These are indexes that span across all shards.
How they work: When data is written, the indexing service builds a separate index (often in another database, like a search engine or a dedicated key-value store) that maps the non-shard-key (e.g., email_address) to the original shard key and shard ID.

* Denormalization: Sometimes, frequently accessed read-only data is duplicated across shards or stored in a separate data store to avoid cross-shard joins or scatter-gather queries.

