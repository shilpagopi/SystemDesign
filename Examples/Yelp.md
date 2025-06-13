# Yelp

### FR
* users can search for entities (entitytype, location, count) within searchradius from curr location
* users can post reviews (text)

### NFR
* availability>>consistency
* low latency search
* reliability, durability

OOS:
* Filters: recently rated.. latest, ratings
* Multimodal reviews
* authentication
* rate limiting

### Core Data Entities
* users (userid - indexed, metadata)
* entities (entityid - indexed, entitytype, location - lat, long, ratings) - shard by entitytypes?
* reviews (reviewid, entityid, reviewcontenturl, reviewtimestamp)

### APIs
* search(currentuserlocation, entitytype, searchradius, count) -> paginated results
* review(userid, entityid, reviewcontent, timestamp)

### BOE
* ready heavy 1:500
