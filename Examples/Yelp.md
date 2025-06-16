# Yelp

### FR
* users can search for entities (name, entitytype, location, count) within searchradius from curr location
* users can post reviews (text) about businesses
* users can view businesses

### NFR
* availability>>consistency
* low latency search
* reliability, durability, scalability
* each user can only leave one review for a business

OOS:
* Filters: recently rated.. latest, ratings
* Multimodal reviews
* authentication
* rate limiting

### Core Data Entities
* users (userid - indexed, metadata)
* entities (entityid - indexed, entitytype, location - lat, long, avgrating, totalratings) - shard by entitytypes?
* reviews (reviewid, userid, entityid, reviewcontenturl, reviewtimestamp, reviewrating) - unique key on userid, entityid
SQL (POSTGRES) for as relationships/joins.

### APIs
* search(currentuserlocation, entitytype, searchradius, count) -> paginated results for list[businesses]
* review(userid, entityid, reviewcontent) (POST businesses/:id/reviews/) (need user auth)
* GET businesses/:id -> Business Details
* GET businesses/:id/reviews?page={}&limit={} -> Reviews[] -> paginated results

### BOE
* ready heavy 1:500
* 100M DAU, 10M businesses, say user posts 1 comment a day, 100M comments/10^5 seconds a day/500(ratio)=> Write QPS: 2 queries/sec. Read QPS: 1000

### HLD
* search business service: handle search and view business
* review service
  * validate ratings, optional text
  * user authentication
  * update avgRating column, and numRatings in entity table directly for each new review (atomicity by using transactions) (if too many reviews, add a message queue for reviews and update later. Else, schedule a cron job for say, a daily update)

### Deepdives
* Search
  * Elastic Search : elastic search is a NoSql db (document store). We need a CDC between actual database and elastic seach DB. CDC acts a change stream, consumed by a worker to update elastic search db. Could be an overkill for this problem.
  * PostGres supports full text search
  * POSTGIS for geospatial indexing (Alternative: Use QuadTrees as entity locations are mostly unchanging)
* Avg Ratings

### Diagram
<img width="939" alt="image" src="https://github.com/user-attachments/assets/5486605b-54f3-431e-9e40-03595a2b783b" />
P.S. Need not use elastic search, use POSTGIS extensions and GiST indexes.

### Follow-ups
###### How to ensure consistency or prevent race conditions during business rating update?
  * Row-wise locking or pessimistic locking
  * Optimistic concurrency control (check avgrating, and numratings column versions at the beginning and end of the transaction), else rollback and retry. Can bs used as system throughput is less.
 
###### How to ensure 1 user only posts 1 review for a business? 
Add validation check in review service, else add composite unique key for userid,entityid in review table. Another alternative is to add this check in the client app UI (but people could bypass that via direct API call). Better approach

##### How to optimize multicriteria queries on Elastic Search?
Use query profiler, use enums for entity type


