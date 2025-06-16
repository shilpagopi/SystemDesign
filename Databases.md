# Databases
~ Decision parameters: Structured data? Consistency? Read/Write heavy?

##### SQL vs. NoSQL
Param|SQL|NoSQL|
--|--|--
Consistency|high-consistency|eventual consistency
ACID| stronger ACID guarantees (atomicity, consistency, isolation, durability)|
Schema|fixed|flexible
Querying|powerful inbuilt querying|limited in the types of efficient queries possible
Writes|slower (SSD erases and rewrites take time)|faster (uses a log structure that only appends existing data)
Reads|faster|slower
Latency|larger to ensure consistency via locking mechanisms|
Underlying Mechanism| B-Trees, used in SQL DBs, are slower to write into. In SQL databases data is stored in pages, each page is traditionally 4 KB in size and maps onto a specific sector of hard drive space. Once a page becomes too large, this page is repartitioned to point to new children pages and the values get sorted into them. SSD has to erase and rewrite - taking time.|Mostly operates with the underlying data structure of a Log-structured merge tree (LSMT) and SS-tables (read more).
sharding and scaling | (maybe)|Managed NoSQL services like DynamoDB or Azure MongoDB provide them
  
##### Types of NoSQL databases
* Key Value: these are the most popular type opaque to the content. scalable by sharding of data, by default is eventually consistent
* Document databases: can perform aggregate searches across data, and store in a variety of formats like JSON, XML, and YAML.
* Columnar databases: Stores information in tables but allows to have denormalized data. The indexing is based on columns rather than rows.
* Graph databases: to store complicated node and edge relationships, allows for easy graph transversal and modification.

##### Elastic Search
Elastic search is like a document store for efificient searches. Use CDC (change stream consumed by worker) to maintain consistency between actual primary database and elastic search. (Not preferred for joins, use denormalize data if atmost necessary)

##### DBs for Location Search
* POSTGIS - open-source spatial database extender for PostgreSQL. Supported under the hood by GiST indexes
* GiST (Generalized Search Tree) is not a single type of index like a B-tree or Hash index, but rather a general-purpose, extensible indexing framework within PostgreSQL. It abstracts the common functionalities of a tree-based index (like balancing, searching, inserting, deleting, and handling concurrency) from the specific logic of how a particular data type is indexed and how queries are performed on it. GiST can be adapted to index many other non-traditional data types, such as:
Full-text search (tsvector/tsquery), Arrays (e.g., btree_gist for B-tree like behavior on arrays), Range types (e.g., daterange, int4range), Multidimensional cubes (cube extension), Tree-like hierarchies (ltree extension)
<img width="731" alt="image" src="https://github.com/user-attachments/assets/09dbf28c-ba82-4b2d-88bd-e53820db982f" />

##### Bigtable
When suggesting Bigtable, make sure to explain why it's a good fit by referencing its core characteristics:

* Massive Scale: Handles terabytes to petabytes of data.
* High Throughput & Low Latency: Designed for millions of reads/writes per second with millisecond latency.
* Wide-Column Store: Flexible schema, ideal for semi-structured data with varying columns per row.
* Sorted Row Keys: Data is lexicographically sorted by row key, enabling efficient range scans. This is crucial for time-series and analytical rangewise queries.
* Atomic Row-Level Operations: All reads and writes within a single row are atomic.
* Versions: Can store multiple timestamped versions of data in a single cell.
* Managed Service: Reduces operational overhead compared to self-managing HBase.
* HBase API Compatibility: Existing HBase applications can often migrate easily.

When NOT to use Bigtable:
* Relational Data: Not suitable for highly normalized, relational data that requires complex joins or multi-row transactions. Use a relational database (e.g., Cloud SQL, Spanner) instead.
* Ad-hoc Queries: While it's fast for row key or range-based queries, it's not a good fit for ad-hoc, complex SQL-like analytical queries across the entire dataset. For that, use a data warehouse like BigQuery.
* Small Datasets: Overkill and more expensive for small to medium-sized datasets.
* Complex Transactions: Only supports single-row transactions. Multi-row transactions require application-level coordination.
