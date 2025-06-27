# Solution Track
* Requirements: functional and non-functional
   * Identify the main business objects and their relations
   * Data types: Structured data, Media and blobs (jar,tar files, binary data)
   * APIs: access patterns: Given [object A], get all related [object B]
  * What information do these objects hold? Can it contain media?
  * mutability of objects: editing, deletion
  * NFR: Performance (optimize for user facing sync requests), partition tolerance, Availability, Consistency, Security (eg. running low trust user submitted code in isolation)
* Scale: Read and write, QPS and Data storage
* Design: Microservices
  
## For Product Design
<img width="1117" alt="image" src="https://github.com/user-attachments/assets/3cbfa577-296a-4829-be25-a40d36c779fb" />

## For Infrastructure Design
<img width="1572" alt="image" src="https://github.com/user-attachments/assets/f0c24130-1ab5-488e-982b-66479883b951" />

## Deepdive CheatSheet
No. | Problem Statement | Deepdives | Remarks
--|--|--|--
1| Dropbox | * Chunking: Multi-part upload <br> * Sync: Delta sync, fingerprinting, reconciliation| * Intelligent client side app <br> * Sync frequency: adaptive polling based on client usage
2 | BookMyShow / TicketMaster | * Elastic Search with location and other criteria search<br> * Distributed Lock for temp reserved seats <br>  * Virtual waiting queue (kafka queue or a redis sortedset based on incoming timestamp) for selected events to handle high surge. | Caches for quick event detail lookups and searches. <br> SSE Connection to continuously update reserved statusis in ticket layout page|
3 | Uber | * Location - Redis Geohashing <br> * Consistent Ride matching - Use distributed lock for maintaining reserved state, attempt one driver at a time (say for 10s) | High surges - Queue |
4| Twitter | * Newsfeed Fan-out-write and Fan-out read, newsfeed cache <br> * Search: Elastic Search| PostID+EpochTime (For 50 years, 86400 sec/day * 365 (days a year) * 50 (years) => 1.6 billion seconds We would need 31 bits to store this number.)
5|Ad Click Aggregator|* Apache Flink - for realtime stream data aggregation from queues <br> * OLAP databases for analytical processing|* ad impression ids instead of Ad ids to handle repetition and prevent user abuse <br> * Reconciliation module based on Apache Spark Map Reduce<br> * checkpointing <br> * queues with retention
6| Whatsapp | * Websocket connections * Online/Offline status | Cleanup job on undelivered messages|
7| Instagram or Twitter | * Photo ID creation with timestamp * Hybrid: Fan-out on read (for celebrity accounts), Fan-out on write * Search: Elastic search| Follow - GSI creation|
8| TinyURL/PasteBin | * Unique ID Generation (MD5 (128bit), SHA (256bit), Key Generation Service) base 64 encoding 6letter~68B | * Support custom alias and TTL <br> * Cache |
9| Twitter Search | * Likewise or Time wise search: maintain two indices *  Tokenization | * likes aggregator - write only if likes change by log to the base 2 <br> * Move cold indices to blob storage |
10| Uber | * Location update/search: Use Redis and its Geohashing <br> * Ride matching consistency: Use distributed lock (Dynamo DB) of drivers in "reserved" status. | Optimize for updating live cab locations on driverclient side|
11| Yelp | PostGREs with GIS (tree structure for not dynamic location) (Elastic Search maybe an overkill)| * Use row-wise locking for avg ratings, numRatings columns OR Optimistic concurrency control (check avgrating, and numratings column versions at the beginning and end of the transaction), else rollback and retry. <br> * Use <userid,reviewid> composite key to prevent users from reviewing multiple times. |

To Add: 
Bidding
TypeAhead
TopK
Youtube
Tinder

## BOE Estimates
No. | Problem Statement |Users | Entities
1| Dropbox |  | 

## Location Search 
<img width="748" alt="image" src="https://github.com/user-attachments/assets/c550defa-dfff-443e-9d32-e3ecfd345799" />


## Tips
* For location search: if there are other mulitple filters, go for elastic search and its location support. Else go for postGRES and PostGIS.
* PostGRES can maybe handle 5k transactions/sec (say max upto 10k)
* Redis and Kafka can handle a 100k-1M Transactions
