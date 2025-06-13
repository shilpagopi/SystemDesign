# Yelp

### FR
* users can search for entities (entitytype, location, count)
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
* entities (entityid - indexed, entitytype, location - lat, long, ratings)
* reviews (reviewid, entityid, reviewcontenturl, reviewtimestamp)

### APIs
* search(searchstring, entitytype, location, count)
* review(entity, )

### BOE
* ready heavy 1:500
