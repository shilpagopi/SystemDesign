# Instagram
### FR
* Follow
* Createposts (text,images)
* Feed

OOS:
* shortvideos
* resticted access
* analytics
* authentication
* searches and hashtags
* liking, commenting, follow recommendations

### NFR
* Read-heavy
* Availability>>Consistency
* Reliability
* Low latency feed

### Data Entities
* user.. metadata
* postid, postcontenturl, userid, ...
* follower,followee (indexed by followee)

### BOE
* Read-heavy 
* Estimates: 500M total users, with 1M daily active users. 2M new photos every day, 23 new photos every second.
* 
