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
  
