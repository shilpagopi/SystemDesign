# Databases
##### SQL
* Use for high-consistency requirement
* powerful in-built querying
* stronger ACID guarantees (atomicity, consistency, isolation, durability)
* B-Trees, used in SQL DBs, are slower to write into. In SQL databases data is stored in pages, each page is traditionally 4 KB in size and maps onto a specific sector of hard drive space. Once a page becomes too large, this page is repartitioned to point to new children pages and the values get sorted into them. SSD has to erase and rewrite - taking time.
* Larger latency for to ensure consistency via locking mechanisms

##### NoSQL  
* Use for eventual consistency requirement
* slower reads than SQL
* Faster writes: uses a log structure that only appends existing data.
* Can handle flexible schema
