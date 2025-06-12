# TypeAhead
### FR
* top k (k = 10) suggestions while user types in search queries

OOS: 
* Analytics
* Ranking crtieria other than frequency: freshness, user location, language, demographics, personal history etc.

### NFR
* low latency <200ms
* Case sensitivity
* no duplicates

### Data Entities
* search queries in the form of a compressed trie

### APIs
HTTP/2 (Good Candidate)
How it works: HTTP/2 introduces multiplexing, allowing multiple requests and responses to be sent concurrently over a single persistent TCP connection. It also has header compression (HPACK) and server push.

Efficiency:
Reduced latency: Reuses the same TCP connection, eliminating the overhead of repeated handshakes.
Multiplexing: If a user types very fast, multiple requests can be "in flight" simultaneously without blocking each other. This is crucial for a responsive typeahead.
Header compression: Reduces the size of request and response headers, saving bandwidth.
Verdict: Very efficient for typeahead. It significantly improves upon HTTP/1.1 by reducing latency and overhead. This is often the most practical and widely adopted solution for browser-based typeahead.

### BOE
* Estimates: Searches: 5B/day, 60K queries per second.
* Insertion: 100M unique queries/day, avg query length: 15char, 30 bytes, 3 GB/day. Assuming new queries are like 2% per day: 25 GB/year
* our index can easily fit on one server. Still we could try for partitioning based on sending some ranges : say, A-AABC on one node, whichever subtree does not fit into the server and maintain range:server mapping. For query "AA" search both servers, for query "AAA" search server 1.
  
### HLD and Deepdives
* Search: at each node,store top k suggestions (node pointers), reverse traverse to last node user has typed in, to recreate the string
* Update:
  * Batch update say every hour, using map reduce processes for queries with frequency>say 10000
  * We can have a master-slave configuration for each trie server. We can update slave while the master is serving traffic. Once the update is complete, we can make the slave our
  new master. We can later update our old master, which can then start serving traffic, too.
  * Inserting a new query node with delta frequency: update the terminal node and traverse all parents to update top k list.
  * Frequencies can be updated as exponential moving average to give weightage to new terms
  * Pruning: least recently used or lowest frequency: Remove the node and its references form all parent nodes.
  * For immediate removal, apply a post processing filter module
* Checkpointing of tries: Serialization: pre-order traversal with number of children. Eg.C2,A1,P0,O1,T0
* Client app:
  * Call typeahead only if the user does not type for say, 50ms.
  * Cancel the inprogress call
  * Setup the connection


